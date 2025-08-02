--[[
Blade Ball Script: Auto Parry + Painel GUI (Mover, Fechar, Fusão)
Feito por Copilot

ATENÇÃO:
- Use com cuidado, scripts de terceiros podem levar a banimentos.
- Adapte a função de "fusão" conforme sua habilidade.
]]

-- Função de auto parry (defender bola automaticamente)
local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local LocalPlayer = Players.LocalPlayer

local autoParry = false
local autoFusion = false

-- Função de Parry (ajuste para o nome do RemoteEvent do parry caso não funcione)
local function doParry()
    local parryRemote = ReplicatedStorage:FindFirstChild("Remotes"):FindFirstChild("ParryAttempt")
    if parryRemote then
        parryRemote:FireServer()
    end
end

-- Função de Fusão (ATENÇÃO: ajuste o nome do RemoteEvent para fusão da sua habilidade)
local function doFusion()
    local fusionRemote = ReplicatedStorage:FindFirstChild("Remotes"):FindFirstChild("FusionAttempt")
    if fusionRemote then
        fusionRemote:FireServer()
    end
end

-- Detecta a bola vindo
local function onHeartbeat()
    if not autoParry then return end
    for _, obj in pairs(workspace:GetChildren()) do
        if obj.Name == "Ball" and obj:IsA("BasePart") then
            local distance = (obj.Position - LocalPlayer.Character.HumanoidRootPart.Position).Magnitude
            if distance < 20 then -- Ajuste a distância se necessário
                doParry()
            end
        end
    end
end

-- Loop de detecção
game:GetService("RunService").Heartbeat:Connect(onHeartbeat)

-- GUI
local ScreenGui = Instance.new("ScreenGui", game.CoreGui)
ScreenGui.Name = "BladeBallPanel"

local Frame = Instance.new("Frame", ScreenGui)
Frame.Size = UDim2.new(0, 240, 0, 140)
Frame.Position = UDim2.new(0, 50, 0, 200)
Frame.BackgroundColor3 = Color3.fromRGB(24, 24, 32)
Frame.BorderSizePixel = 0
Frame.Active = true
Frame.Draggable = true

local title = Instance.new("TextLabel", Frame)
title.Text = "BLADE BALL PAINEL"
title.Font = Enum.Font.SourceSansBold
title.TextSize = 22
title.Size = UDim2.new(1, 0, 0, 36)
title.BackgroundTransparency = 1
title.TextColor3 = Color3.fromRGB(150, 255, 180)

local btnAuto = Instance.new("TextButton", Frame)
btnAuto.Text = "Auto Parry: OFF"
btnAuto.Font = Enum.Font.SourceSansBold
btnAuto.TextSize = 19
btnAuto.Size = UDim2.new(1, -30, 0, 32)
btnAuto.Position = UDim2.new(0, 15, 0, 42)
btnAuto.BackgroundColor3 = Color3.fromRGB(50, 60, 80)
btnAuto.TextColor3 = Color3.fromRGB(255, 255, 255)
btnAuto.MouseButton1Click:Connect(function()
    autoParry = not autoParry
    btnAuto.Text = "Auto Parry: " .. (autoParry and "ON" or "OFF")
    btnAuto.BackgroundColor3 = autoParry and Color3.fromRGB(0,180,100) or Color3.fromRGB(50,60,80)
end)

local btnFusion = Instance.new("TextButton", Frame)
btnFusion.Text = "Fusão: OFF"
btnFusion.Font = Enum.Font.SourceSansBold
btnFusion.TextSize = 19
btnFusion.Size = UDim2.new(1, -30, 0, 32)
btnFusion.Position = UDim2.new(0, 15, 0, 82)
btnFusion.BackgroundColor3 = Color3.fromRGB(80, 40, 120)
btnFusion.TextColor3 = Color3.fromRGB(255, 255, 255)
btnFusion.MouseButton1Click:Connect(function()
    autoFusion = not autoFusion
    btnFusion.Text = "Fusão: " .. (autoFusion and "ON" or "OFF")
    btnFusion.BackgroundColor3 = autoFusion and Color3.fromRGB(180,60,220) or Color3.fromRGB(80,40,120)
    if autoFusion then
        doFusion()
    end
end)

-- MOVER PAINEL
local posX = 50
local btnLeft = Instance.new("TextButton", Frame)
btnLeft.Text = "<"
btnLeft.Font = Enum.Font.SourceSansBold
btnLeft.TextSize = 20
btnLeft.Size = UDim2.new(0, 35, 0, 28)
btnLeft.Position = UDim2.new(0, 0, 1, -38)
btnLeft.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
btnLeft.TextColor3 = Color3.fromRGB(255,255,255)
btnLeft.MouseButton1Click:Connect(function()
    posX = posX - 60
    Frame.Position = UDim2.new(0, posX, 0, 200)
end)

local btnRight = Instance.new("TextButton", Frame)
btnRight.Text = ">"
btnRight.Font = Enum.Font.SourceSansBold
btnRight.TextSize = 20
btnRight.Size = UDim2.new(0, 35, 0, 28)
btnRight.Position = UDim2.new(1, -35, 1, -38)
btnRight.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
btnRight.TextColor3 = Color3.fromRGB(255,255,255)
btnRight.MouseButton1Click:Connect(function()
    posX = posX + 60
    Frame.Position = UDim2.new(0, posX, 0, 200)
end)

-- FECHAR PAINEL
local btnClose = Instance.new("TextButton", Frame)
btnClose.Text = "X"
btnClose.Font = Enum.Font.SourceSansBold
btnClose.TextSize = 20
btnClose.Size = UDim2.new(0, 35, 0, 28)
btnClose.Position = UDim2.new(1, -35, 0, 0)
btnClose.BackgroundColor3 = Color3.fromRGB(200, 60, 60)
btnClose.TextColor3 = Color3.fromRGB(255,255,255)
btnClose.MouseButton1Click:Connect(function()
    ScreenGui:Destroy()
end)
