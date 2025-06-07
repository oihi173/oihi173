-- Services
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")

-- Local player
local localPlayer = Players.LocalPlayer

-- GUI Setup
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "ESP_GUI"
ScreenGui.ResetOnSpawn = false
ScreenGui.Parent = localPlayer:WaitForChild("PlayerGui")

local ToggleButton = Instance.new("TextButton")
ToggleButton.Size = UDim2.new(0, 100, 0, 30)
ToggleButton.Position = UDim2.new(0, 10, 0, 10)
ToggleButton.Text = "ESP: OFF"
ToggleButton.Parent = ScreenGui
ToggleButton.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
ToggleButton.TextColor3 = Color3.fromRGB(255, 255, 255)

-- ESP Settings
local espEnabled = false
local espBoxes = {}

-- Toggle Function
ToggleButton.MouseButton1Click:Connect(function()
    espEnabled = not espEnabled
    ToggleButton.Text = espEnabled and "ESP: ON" or "ESP: OFF"

    if not espEnabled then
        for _, box in pairs(espBoxes) do
            box:Destroy()
        end
        espBoxes = {}
    end
end)

-- Function to create name tag
local function createESP(player)
    local character = player.Character or player.CharacterAdded:Wait()
    local head = character:WaitForChild("Head")

    local billboard = Instance.new("BillboardGui")
    billboard.Adornee = head
    billboard.Size = UDim2.new(0, 100, 0, 40)
    billboard.StudsOffset = Vector3.new(0, 2, 0)
    billboard.AlwaysOnTop = true
    billboard.Name = player.Name .. "_ESP"
    billboard.Parent = ScreenGui

    local textLabel = Instance.new("TextLabel")
    textLabel.Size = UDim2.new(1, 0, 1, 0)
    textLabel.BackgroundTransparency = 1
    textLabel.TextColor3 = Color3.fromRGB(255, 0, 0)
    textLabel.TextStrokeTransparency = 0.5
    textLabel.Text = player.Name
    textLabel.Parent = billboard

    espBoxes[player] = billboard
end

-- Main Loop
RunService.RenderStepped:Connect(function()
    if not espEnabled then return end

    for _, player in pairs(Players:GetPlayers()) do
        if player ~= localPlayer and player.Team ~= localPlayer.Team then
            if not espBoxes[player] then
                createESP(player)
            end
        else
            if espBoxes[player] then
                espBoxes[player]:Destroy()
                espBoxes[player] = nil
            end
        end
    end
end)

-- Cleanup on player leaving
Players.PlayerRemoving:Connect(function(player)
    if espBoxes[player] then
        espBoxes[player]:Destroy()
        espBoxes[player] = nil
    end
end)
