local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local RunService = game:GetService("RunService")

-- GUI Setup
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Parent = game.CoreGui

local Panel = Instance.new("Frame")
Panel.Size = UDim2.new(0, 200, 0, 250)
Panel.Position = UDim2.new(0, 50, 0, 100)
Panel.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
Panel.Visible = false
Panel.Parent = ScreenGui

local ToggleBtn = Instance.new("TextButton")
ToggleBtn.Size = UDim2.new(0, 100, 0, 40)
ToggleBtn.Position = UDim2.new(0, 0, 0, 0)
ToggleBtn.Text = "Abrir Painel"
ToggleBtn.BackgroundColor3 = Color3.fromRGB(70, 130, 180)
ToggleBtn.Parent = ScreenGui

-- Painel Options (Auto Parry, ESP)
local AutoParryToggle = Instance.new("TextButton")
AutoParryToggle.Size = UDim2.new(0, 180, 0, 40)
AutoParryToggle.Position = UDim2.new(0, 10, 0, 60)
AutoParryToggle.Text = "Auto Parry [OFF]"
AutoParryToggle.BackgroundColor3 = Color3.fromRGB(60, 60, 110)
AutoParryToggle.Parent = Panel

local ESPToggle = Instance.new("TextButton")
ESPToggle.Size = UDim2.new(0, 180, 0, 40)
ESPToggle.Position = UDim2.new(0, 10, 0, 120)
ESPToggle.Text = "ESP [OFF]"
ESPToggle.BackgroundColor3 = Color3.fromRGB(60, 110, 60)
ESPToggle.Parent = Panel

-- Variables
local panelOpen = false
local autoParry = false
local espOn = false
local espObjects = {}

-- Toggle Panel
ToggleBtn.MouseButton1Click:Connect(function()
    panelOpen = not panelOpen
    Panel.Visible = panelOpen
    ToggleBtn.Text = panelOpen and "Fechar Painel" or "Abrir Painel"
end)

-- Auto Parry Logic
function doParry()
    -- Tenta encontrar a bola e rebatê-la (ajuste para Blade Ball)
    local ball = workspace:FindFirstChild("Ball")
    if ball and ball:FindFirstChild("Target") and ball.Target.Value == LocalPlayer then
        -- Comando para usar parry
        local remote = ReplicatedStorage:FindFirstChild("Remotes"):FindFirstChild("ParryAttempt")
        if remote then remote:FireServer() end
    end
end

AutoParryToggle.MouseButton1Click:Connect(function()
    autoParry = not autoParry
    AutoParryToggle.Text = "Auto Parry ["..(autoParry and "ON" or "OFF").."]"
end)

-- ESP Logic
function addESP(player)
    if player.Character and player.Character:FindFirstChild("Head") and not espObjects[player] then
        local billboard = Instance.new("BillboardGui")
        billboard.Name = "ESP"
        billboard.Size = UDim2.new(0, 100, 0, 30)
        billboard.Adornee = player.Character.Head
        billboard.AlwaysOnTop = true

        local label = Instance.new("TextLabel")
        label.Size = UDim2.new(1, 0, 1, 0)
        label.Text = player.Name
        label.TextColor3 = Color3.new(1,0,0)
        label.BackgroundTransparency = 1
        label.Parent = billboard

        billboard.Parent = workspace
        espObjects[player] = billboard
    end
end

function removeESP(player)
    if espObjects[player] then
        espObjects[player]:Destroy()
        espObjects[player] = nil
    end
end

ESPToggle.MouseButton1Click:Connect(function()
    espOn = not espOn
    ESPToggle.Text = "ESP ["..(espOn and "ON" or "OFF").."]"
    if not espOn then
        for _, esp in pairs(espObjects) do esp:Destroy() end
        espObjects = {}
    else
        for _, plr in pairs(Players:GetPlayers()) do
            if plr ~= LocalPlayer then addESP(plr) end
        end
    end
end)

Players.PlayerAdded:Connect(function(plr)
    plr.CharacterAdded:Connect(function()
        if espOn and plr ~= LocalPlayer then addESP(plr) end
    end)
end)

Players.PlayerRemoving:Connect(function(plr)
    removeESP(plr)
end)

-- Main Loop
RunService.RenderStepped:Connect(function()
    if autoParry then doParry() end
    if espOn then
        for _, plr in pairs(Players:GetPlayers()) do
            if plr ~= LocalPlayer then addESP(plr) end
        end
    end
end)
