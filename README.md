local ScreenGui = Instance.new("ScreenGui")
local MainFrame = Instance.new("Frame")
local TitleBar = Instance.new("Frame")
local TitleLabel = Instance.new("TextLabel")
local CloseButton = Instance.new("TextButton")
local OpenButton = Instance.new("TextButton")
local Dragging = false
local DragInput, MousePos, FramePos

ScreenGui.Name = "WildHorseScriptUI"
ScreenGui.Parent = game:GetService("Players").LocalPlayer:WaitForChild("PlayerGui")

-- Open/Close Button (initially only open shown, main frame hidden)
OpenButton.Name = "OpenButton"
OpenButton.Text = "Open VIPER Panel"
OpenButton.Size = UDim2.new(0, 150, 0, 30)
OpenButton.Position = UDim2.new(0, 10, 0, 10)
OpenButton.Parent = ScreenGui

MainFrame.Name = "MainFrame"
MainFrame.Size = UDim2.new(0, 350, 0, 450)
MainFrame.Position = UDim2.new(0, 200, 0, 100)
MainFrame.BackgroundColor3 = Color3.fromRGB(30,30,30)
MainFrame.BorderSizePixel = 0
MainFrame.Visible = false
MainFrame.Active = true
MainFrame.Parent = ScreenGui

TitleBar.Name = "TitleBar"
TitleBar.Size = UDim2.new(1,0,0,30)
TitleBar.BackgroundColor3 = Color3.fromRGB(50,50,50)
TitleBar.Parent = MainFrame

TitleLabel.Name = "TitleLabel"
TitleLabel.Text = "VIPER Wild Horse Panel"
TitleLabel.Size = UDim2.new(1, -40, 1, 0)
TitleLabel.Position = UDim2.new(0,10,0,0)
TitleLabel.BackgroundTransparency = 1
TitleLabel.TextColor3 = Color3.fromRGB(255,255,255)
TitleLabel.Font = Enum.Font.GothamBold
TitleLabel.TextSize = 16
TitleLabel.Parent = TitleBar

CloseButton.Name = "CloseButton"
CloseButton.Text = "X"
CloseButton.Size = UDim2.new(0,30,1,0)
CloseButton.Position = UDim2.new(1,-30,0,0)
CloseButton.BackgroundColor3 = Color3.fromRGB(200,50,50)
CloseButton.TextColor3 = Color3.fromRGB(255,255,255)
CloseButton.Font = Enum.Font.GothamBold
CloseButton.TextSize = 14
CloseButton.Parent = TitleBar

-- Drag Panel Logic
TitleBar.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        Dragging = true
        DragInput = input
        MousePos = input.Position
        FramePos = MainFrame.Position
    end
end)
TitleBar.InputChanged:Connect(function(input)
    if Dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
        local delta = input.Position - MousePos
        MainFrame.Position = UDim2.new(FramePos.X.Scale, FramePos.X.Offset + delta.X, FramePos.Y.Scale, FramePos.Y.Offset + delta.Y)
    end
end)
game:GetService("UserInputService").InputEnded:Connect(function(input)
    if input == DragInput then
        Dragging = false
    end
end)

-- Open/Close Actions
OpenButton.MouseButton1Click:Connect(function()
    MainFrame.Visible = true
    OpenButton.Visible = false
end)
CloseButton.MouseButton1Click:Connect(function()
    MainFrame.Visible = false
    OpenButton.Visible = true
end)

-- Utility
local function notify(msg)
    game.StarterGui:SetCore("SendNotification", {
        Title = "VIPER Script", 
        Text = msg
    })
end

-- Button Factory
local function createButton(text, y, callback)
    local btn = Instance.new("TextButton")
    btn.Text = text
    btn.Size = UDim2.new(0.9, 0, 0, 30)
    btn.Position = UDim2.new(0.05, 0, 0, y)
    btn.BackgroundColor3 = Color3.fromRGB(70,70,70)
    btn.TextColor3 = Color3.fromRGB(255,255,255)
    btn.Font = Enum.Font.Gotham
    btn.TextSize = 14
    btn.Parent = MainFrame
    btn.MouseButton1Click:Connect(callback)
    return btn
end

local btnY = 40
local btnSpacing = 35

-- 1. Main
createButton("Auto Farm Horses", btnY, AutoFarmHorses)
btnY = btnY + btnSpacing
createButton("Auto Sell Items", btnY, AutoSellItems)
btnY = btnY + btnSpacing
createButton("Auto Collect Resources", btnY, AutoCollectResources)
btnY = btnY + btnSpacing
createButton("Auto Complete Quests", btnY, AutoCompleteQuests)
btnY = btnY + btnSpacing

-- 2. Item Mods
createButton("Reduce Lasso Cooldown", btnY, ReduceLassoCooldown)
btnY = btnY + btnSpacing
createButton("Expand Capture Radius", btnY, ExpandCaptureRadius)
btnY = btnY + btnSpacing
createButton("Disable Travel Cost", btnY, DisableTravelCost)
btnY = btnY + btnSpacing
createButton("Disable Level Requirement", btnY, DisableLevelRequirement)
btnY = btnY + btnSpacing

-- 3. ESP/Chams
createButton("ESP Horses", btnY, ESPHorses)
btnY = btnY + btnSpacing
createButton("ESP Rarity", btnY, ESPRarity)
btnY = btnY + btnSpacing
createButton("ESP Players", btnY, ESPPlayers)
btnY = btnY + btnSpacing
createButton("ESP Items", btnY, ESPItems)
btnY = btnY + btnSpacing

-- 4. Character
createButton("Fly (Toggle)", btnY, function() Fly(true) end)
btnY = btnY + btnSpacing
createButton("Noclip (Toggle)", btnY, function() Noclip(true) end)
btnY = btnY + btnSpacing
createButton("Speed Hack +10", btnY, function() SpeedHack(10) end)
btnY = btnY + btnSpacing
createButton("Jump Power +50", btnY, function() JumpPower(50) end)
btnY = btnY + btnSpacing
createButton("Godmode (Toggle)", btnY, function() Godmode(true) end)
btnY = btnY + btnSpacing

-- 5. Misc
createButton("Anti AFK", btnY, AntiAFK)
btnY = btnY + btnSpacing
createButton("Infinite Tokens", btnY, InfiniteTokens)
btnY = btnY + btnSpacing
createButton("Unlock All Accessories", btnY, UnlockAllAccessories)
btnY = btnY + btnSpacing
createButton("Teleport To Island", btnY, function() 
    -- shows a prompt for island name
    local islandName = game:GetService("Players").LocalPlayer.PlayerGui:FindFirstChild("IslandPrompt")
    if not islandName then
        islandName = Instance.new("TextBox")
        islandName.Name = "IslandPrompt"
        islandName.Text = "Enter Island Name"
        islandName.Size = UDim2.new(0, 200, 0, 30)
        islandName.Position = UDim2.new(0, 75, 0, btnY)
        islandName.Parent = MainFrame
        islandName.FocusLost:Connect(function(enter)
            if enter then
                TeleportToIsland(islandName.Text)
                islandName:Destroy()
            end
        end)
    end
end)
btnY = btnY + btnSpacing
createButton("Save Settings", btnY, SaveSettings)
btnY = btnY + btnSpacing
createButton("Load Settings", btnY, LoadSettings)

-- Feature Functions (same as your template, above)
function AutoFarmHorses()
    notify("Auto Farm Horses activated!")
    -- TODO: Add logic
end

function AutoSellItems()
    notify("Auto Sell Items activated!")
    -- TODO: Add logic
end

function AutoCollectResources()
    notify("Auto Collect Resources activated!")
    -- TODO: Add logic
end

function AutoCompleteQuests()
    notify("Auto Complete Quests activated!")
    -- TODO: Add logic
end

function ReduceLassoCooldown()
    notify("Lasso Cooldown Reduced!")
    -- TODO: Add logic
end

function ExpandCaptureRadius()
    notify("Capture Radius Expanded!")
    -- TODO: Add logic
end

function DisableTravelCost()
    notify("Travel cost disabled!")
    -- TODO: Add logic
end

function DisableLevelRequirement()
    notify("Level requirement disabled!")
    -- TODO: Add logic
end

function ESPHorses()
    notify("ESP Horses enabled!")
    -- TODO: Add logic
end

function ESPRarity()
    notify("ESP Rarity enabled!")
    -- TODO: Add logic
end

function ESPPlayers()
    notify("ESP Players enabled!")
    -- TODO: Add logic
end

function ESPItems()
    notify("ESP Items enabled!")
    -- TODO: Add logic
end

function Fly(toggle)
    notify("Fly toggled: " .. tostring(toggle))
    -- TODO: Add logic
end

function Noclip(toggle)
    notify("Noclip toggled: " .. tostring(toggle))
    -- TODO: Add logic
end

function SpeedHack(amount)
    notify("Speed changed to: " .. tostring(amount))
    -- TODO: Add logic
end

function JumpPower(amount)
    notify("Jump power set to: " .. tostring(amount))
    -- TODO: Add logic
end

function Godmode(toggle)
    notify("Godmode toggled: " .. tostring(toggle))
    -- TODO: Add logic
end

function AntiAFK()
    notify("Anti AFK enabled!")
    local vu = game:GetService("VirtualUser")
    game:GetService("Players").LocalPlayer.Idled:connect(function()
        vu:Button2Down(Vector2.new(0,0),workspace.CurrentCamera.CFrame)
        wait(1)
        vu:Button2Up(Vector2.new(0,0),workspace.CurrentCamera.CFrame)
    end)
end

function InfiniteTokens()
    notify("Infinite Tokens script!")
    -- TODO: Add logic
end

function UnlockAllAccessories()
    notify("Unlock All Accessories script!")
    -- TODO: Add logic
end

function TeleportToIsland(islandName)
    notify("Teleporting to: " .. islandName)
    -- TODO: Add logic
end

function SaveSettings()
    notify("Settings Saved!")
    -- TODO: Add logic
end

function LoadSettings()
    notify("Settings Loaded!")
    -- TODO: Add logic
end

-- Done! 
-- Panel is draggable, can open/close, and buttons connect to functions.
