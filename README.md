-- SISTEMA DE WHITELIST PARA 10PEREIRAZZK
local player = game.Players.LocalPlayer
local allowedNick = "10Pereirazzk"

if player.Name ~= allowedNick then
    player:Kick("Whitelist negada para: " .. player.Name .. ". Este script e privado para 10Pereirazzk.")
    return
end

-- CARREGAMENTO DA INTERFACE (KAVO UI)
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()
local Window = Library.CreateLib("PAINEL SUPREME - 10PEREIRAZZK", "DarkTheme")

-- ABA: PERFORMANCE E TELA
local Tab1 = Window:NewTab("Otimizacao")
local Section1 = Tab1:NewSection("Performance & Visao")

-- INPUT PARA ESTICAR A TELA (FOV)
Section1:NewTextBox("Esticar Tela (FOV)", "Digite o numero (Ex: 130)", function(txt)
    local num = tonumber(txt)
    if num then
        game.Workspace.CurrentCamera.FieldOfView = num
    end
end)

-- FUNCAO NO FOG (REMOVER NEBLINA)
Section1:NewButton("Remover Neblina (No Fog)", "Limpa a visao do mapa", function()
    game.Lighting.FogEnd = 9e9
    for i,v in pairs(game.Lighting:GetChildren()) do
        if v:IsA("Atmosphere") or v:IsA("BloomEffect") or v:IsA("BlurEffect") then
            v:Destroy()
        end
    end
    print("Neblina Removida!")
end)

Section1:NewButton("Modo Batata (FPS Boost)", "Remove texturas pesadas", function()
    local g = game
    settings().Rendering.QualityLevel = 1
    for i, v in pairs(g:GetDescendants()) do
        if v:IsA("Part") or v:IsA("UnionOperation") or v:IsA("MeshPart") then
            v.Material = Enum.Material.Plastic
            v.Reflectance = 0
        elseif v:IsA("Decal") or v:IsA("Texture") then
            v.Transparency = 1
        end
    end
end)

Section1:NewToggle("Black Screen (Economia)", "Desliga o render para Farm", function(state)
    game:GetService("RunService"):Set3dRenderingEnabled(not state)
end)

-- ABA: ESP E VISUAIS
local Tab2 = Window:NewTab("Visual/ESP")
local Section2 = Tab2:NewSection("Rastreadores")

Section2:NewToggle("ESP Jogadores", "Ver players na parede", function(state)
    getgenv().ESP_Players = state
    while getgenv().ESP_Players do
        for _, v in pairs(game.Players:GetPlayers()) do
            if v ~= player and v.Character and v.Character:FindFirstChild("HumanoidRootPart") then
                if not v.Character.HumanoidRootPart:FindFirstChild("ESP") then
                    local b = Instance.new("BillboardGui", v.Character.HumanoidRootPart)
                    b.Name = "ESP"
                    b.AlwaysOnTop = true
                    b.Size = UDim2.new(4,0,5,0)
                    local f = Instance.new("Frame", b)
                    f.Size = UDim2.new(1,0,1,0)
                    f.BackgroundColor3 = Color3.fromRGB(0, 255, 255)
                    f.BackgroundTransparency = 0.5
                end
            end
        end
        wait(1)
        if not getgenv().ESP_Players then
            for _, v in pairs(game.Players:GetPlayers()) do
                if v.Character and v.Character.HumanoidRootPart:FindFirstChild("ESP") then
                    v.Character.HumanoidRootPart.ESP:Destroy()
                end
            end
        end
    end
end)

Section2:NewButton("ESP Frutas", "Localiza frutas no chao", function()
    for i,v in pairs(game.Workspace:GetChildren()) do
        if v:IsA("Tool") or (v:IsA("Model") and string.find(v.Name, "Fruit")) then
            local b = Instance.new("BillboardGui", v:FindFirstChildWhichIsA("BasePart"))
            b.AlwaysOnTop = true
            b.Size = UDim2.new(4,0,4,0)
            local t = Instance.new("TextLabel", b)
            t.Size = UDim2.new(1,0,1,0)
            t.Text = "FRUTA: " .. v.Name
            t.TextColor3 = Color3.new(0,1,0)
            t.BackgroundTransparency = 1
        end
    end
end)

-- ABA: AUTO FARM
local Tab3 = Window:NewTab("Auto Farm")
local Section3 = Tab3:NewSection("Farm Base")

Section3:NewToggle("Auto Click", "Bater sozinho", function(state)
    _G.Attack = state
    while _G.Attack do
        game:GetService("VirtualUser"):Button1Down(Vector2.new(0,1000), game.Workspace.CurrentCamera.CFrame)
        wait(0.1)
    end
end)

-- CREDITOS
local Tab4 = Window:NewTab("Creditos")
local Section4 = Tab4:NewSection("Proprietario: 10Pereirazzk")
Section4:NewLabel("Whitelist: ATIVA ✅")