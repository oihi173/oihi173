local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local Mouse = LocalPlayer:GetMouse()
local UIS = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local Camera = workspace.CurrentCamera

-- // Painel GUI
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "PainelCompleto"
ScreenGui.Parent = game.CoreGui

local Frame = Instance.new("Frame", ScreenGui)
Frame.Size = UDim2.new(0, 250, 0, 330)
Frame.Position = UDim2.new(0, 30, 0, 200)
Frame.BackgroundColor3 = Color3.fromRGB(25,25,25)
Frame.Active = true
Frame.Draggable = true

local UICorner = Instance.new("UICorner", Frame)
UICorner.CornerRadius = UDim.new(0,10)

local Title = Instance.new("TextLabel", Frame)
Title.Text = "Painel Completo"
Title.Size = UDim2.new(1,0,0,40)
Title.BackgroundTransparency = 1
Title.TextColor3 = Color3.new(1,1,1)
Title.Font = Enum.Font.GothamBold
Title.TextSize = 22

local Y = 50 -- Espaço inicial para botões

function CreateButton(text, callback)
    local Button = Instance.new("TextButton", Frame)
    Button.Size = UDim2.new(1, -20, 0, 36)
    Button.Position = UDim2.new(0, 10, 0, Y)
    Button.BackgroundColor3 = Color3.fromRGB(45,45,45)
    Button.Text = text
    Button.TextColor3 = Color3.new(1,1,1)
    Button.Font = Enum.Font.Gotham
    Button.TextSize = 18
    Button.AutoButtonColor = true

    local Corner = Instance.new("UICorner", Button)
    Corner.CornerRadius = UDim.new(0,8)
    Button.MouseButton1Click:Connect(callback)
    Y = Y + 44
    return Button
end

-- // ESP (Player Highlight)
local ESPEnabled = false
local ESPObjects = {}

function CreateESP(player)
    if player ~= LocalPlayer and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
        local adorn = Instance.new("BoxHandleAdornment")
        adorn.Adornee = player.Character.HumanoidRootPart
        adorn.Size = Vector3.new(4,6,2)
        adorn.Color3 = Color3.fromRGB(255,0,0)
        adorn.AlwaysOnTop = true
        adorn.ZIndex = 10
        adorn.Transparency = 0.7
        adorn.Parent = player.Character
        ESPObjects[player] = adorn
    end
end

function RemoveESP(player)
    if ESPObjects[player] then
        ESPObjects[player]:Destroy()
        ESPObjects[player] = nil
    end
end

function ToggleESP()
    ESPEnabled = not ESPEnabled
    if ESPEnabled then
        for _,player in pairs(Players:GetPlayers()) do
            if player ~= LocalPlayer then
                CreateESP(player)
            end
        end
        Players.PlayerAdded:Connect(function(player)
            player.CharacterAdded:Connect(function()
                wait(1)
                if ESPEnabled then
                    CreateESP(player)
                end
            end)
        end)
        Players.PlayerRemoving:Connect(RemoveESP)
    else
        for player,esp in pairs(ESPObjects) do
            esp:Destroy()
        end
        ESPObjects = {}
    end
end

-- // Fly & Unfly
local Flying = false
local flySpeed = 60
local BodyGyro, BodyVelocity

function Fly()
    if Flying then return end
    Flying = true
    local char = LocalPlayer.Character
    if not char or not char:FindFirstChild("HumanoidRootPart") then return end
    BodyGyro = Instance.new("BodyGyro", char.HumanoidRootPart)
    BodyGyro.P = 9e4
    BodyGyro.maxTorque = Vector3.new(9e9, 9e9, 9e9)
    BodyGyro.cframe = char.HumanoidRootPart.CFrame
    BodyVelocity = Instance.new("BodyVelocity", char.HumanoidRootPart)
    BodyVelocity.velocity = Vector3.new(0,0,0)
    BodyVelocity.maxForce = Vector3.new(9e9, 9e9, 9e9)
    
    local function flyLoop()
        while Flying and char and char:FindFirstChild("HumanoidRootPart") do
            local dir = Vector3.new()
            if UIS:IsKeyDown(Enum.KeyCode.W) then dir = dir + (Camera.CFrame.LookVector) end
            if UIS:IsKeyDown(Enum.KeyCode.S) then dir = dir - (Camera.CFrame.LookVector) end
            if UIS:IsKeyDown(Enum.KeyCode.A) then dir = dir - (Camera.CFrame.RightVector) end
            if UIS:IsKeyDown(Enum.KeyCode.D) then dir = dir + (Camera.CFrame.RightVector) end
            if UIS:IsKeyDown(Enum.KeyCode.Space) then dir = dir + Vector3.new(0,1,0) end
            if UIS:IsKeyDown(Enum.KeyCode.LeftControl) then dir = dir - Vector3.new(0,1,0) end
            BodyVelocity.velocity = dir.Unit * flySpeed
            BodyGyro.cframe = Camera.CFrame
            RunService.RenderStepped:Wait()
        end
    end
    coroutine.wrap(flyLoop)()
end

function Unfly()
    Flying = false
    if BodyGyro then BodyGyro:Destroy() BodyGyro = nil end
    if BodyVelocity then BodyVelocity:Destroy() BodyVelocity = nil end
end

-- // Animações
local AnimSet = {
    ["Dança 1"] = "rbxassetid://33796059",
    ["Dança 2"] = "rbxassetid://1838062888",
    ["Dab"] = "rbxassetid://2482632609",
    ["Floss"] = "rbxassetid://2482632009",
    ["T-Pose"] = "rbxassetid://1113750646",
}

function PlayAnimation(animId)
    local char = LocalPlayer.Character
    if char and char:FindFirstChildOfClass("Humanoid") then
        -- Remove animação anterior
        for _,track in pairs(char.Humanoid:GetPlayingAnimationTracks()) do
            track:Stop()
        end
        local anim = Instance.new("Animation")
        anim.AnimationId = animId
        local track = char.Humanoid:LoadAnimation(anim)
        track:Play()
    end
end

-- // Botões Painel
CreateButton("ESP Players (ON/OFF)", ToggleESP)
CreateButton("Fly (Aperte novamente para voar)", Fly)
CreateButton("Unfly", Unfly)

for name,aid in pairs(AnimSet) do
    CreateButton("Animação: "..name, function() PlayAnimation(aid) end)
end

-- // Créditos
local Credits = Instance.new("TextLabel", Frame)
Credits.Text = "by Copilot GPT\nGithub Copilot"
Credits.Size = UDim2.new(1,0,0,35)
Credits.Position = UDim2.new(0,0,1,-35)
Credits.BackgroundTransparency = 1
Credits.TextColor3 = Color3.fromRGB(180,180,180)
Credits.Font = Enum.Font.Gotham
Credits.TextSize = 13

-- // Hotkeys
UIS.InputBegan:Connect(function(input, gpe)
    if gpe then return end
    if input.KeyCode == Enum.KeyCode.F then
        if not Flying then
            Fly()
        else
            Unfly()
        end
    end
end)

-- // Mensagem inicial
game.StarterGui:SetCore("SendNotification", {
    Title = "Painel Completo";
    Text = "Pressione F para Fly/Unfly.";
    Duration = 5;
})
