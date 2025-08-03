local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local Mouse = LocalPlayer:GetMouse()

-- Criar GUI
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "RedzHubPanel"
ScreenGui.Parent = game.CoreGui

local Panel = Instance.new("Frame")
Panel.Name = "Panel"
Panel.Size = UDim2.new(0, 350, 0, 450)
Panel.Position = UDim2.new(0.5, -175, 0.5, -225)
Panel.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
Panel.BorderSizePixel = 0
Panel.Active = true
Panel.Draggable = true
Panel.Parent = ScreenGui

-- Capa Epic Face
local EpicFace = Instance.new("ImageLabel")
EpicFace.Name = "EpicFace"
EpicFace.Size = UDim2.new(0, 128, 0, 128)
EpicFace.Position = UDim2.new(0.5, -64, 0, 10)
EpicFace.BackgroundTransparency = 1
EpicFace.Image = "rbxassetid://10925156039"
EpicFace.Parent = Panel

-- Título
local Title = Instance.new("TextLabel")
Title.Text = "Redz Hub Style"
Title.Size = UDim2.new(1, 0, 0, 32)
Title.Position = UDim2.new(0, 0, 0, 140)
Title.BackgroundTransparency = 1
Title.Font = Enum.Font.GothamBold
Title.TextSize = 28
Title.TextColor3 = Color3.fromRGB(255,255,0)
Title.Parent = Panel

-- Função para criar botões
function CreateButton(text, y, callback)
    local Button = Instance.new("TextButton")
    Button.Text = text
    Button.Size = UDim2.new(0.8, 0, 0, 32)
    Button.Position = UDim2.new(0.1, 0, 0, y)
    Button.BackgroundColor3 = Color3.fromRGB(55,55,55)
    Button.TextColor3 = Color3.fromRGB(255,255,255)
    Button.Font = Enum.Font.Gotham
    Button.TextSize = 20
    Button.Parent = Panel
    Button.MouseButton1Click:Connect(callback)
    return Button
end

-- Funções dos botões
local isESPEnabled = false
local ESPInstances = {}

function EnableESP()
    isESPEnabled = true
    for _,plr in pairs(Players:GetPlayers()) do
        if plr ~= LocalPlayer and plr.Character then
            local highlight = Instance.new("Highlight")
            highlight.Name = "ESP"
            highlight.Adornee = plr.Character
            highlight.Parent = plr.Character
            highlight.FillColor = Color3.new(1,1,0)
            table.insert(ESPInstances, highlight)
        end
    end
end

function DisableESP()
    isESPEnabled = false
    for _,plr in pairs(Players:GetPlayers()) do
        if plr.Character and plr.Character:FindFirstChild("ESP") then
            plr.Character.ESP:Destroy()
        end
    end
    ESPInstances = {}
end

function Fly()
    local Humanoid = LocalPlayer.Character and LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
    if not Humanoid then return end
    local bp = Instance.new("BodyPosition", LocalPlayer.Character.HumanoidRootPart)
    bp.Name = "FlyBP"
    bp.MaxForce = Vector3.new(1e5,1e5,1e5)
    bp.Position = LocalPlayer.Character.HumanoidRootPart.Position
    local bg = Instance.new("BodyGyro", LocalPlayer.Character.HumanoidRootPart)
    bg.Name = "FlyBG"
    bg.MaxTorque = Vector3.new(1e5,1e5,1e5)
    -- Simples: Pressione W/A/S/D para mover
    local flying = true
    game:GetService("RunService").Heartbeat:Connect(function()
        if flying and LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
            local pos = LocalPlayer.Character.HumanoidRootPart.Position
            if UserInputService:IsKeyDown(Enum.KeyCode.W) then
                bp.Position = pos + (workspace.CurrentCamera.CFrame.lookVector * 2)
            end
            if UserInputService:IsKeyDown(Enum.KeyCode.S) then
                bp.Position = pos - (workspace.CurrentCamera.CFrame.lookVector * 2)
            end
            if UserInputService:IsKeyDown(Enum.KeyCode.A) then
                bp.Position = pos - (workspace.CurrentCamera.CFrame.rightVector * 2)
            end
            if UserInputService:IsKeyDown(Enum.KeyCode.D) then
                bp.Position = pos + (workspace.CurrentCamera.CFrame.rightVector * 2)
            end
        end
    end)
end

function UnFly()
    local root = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
    if root then
        if root:FindFirstChild("FlyBP") then root.FlyBP:Destroy() end
        if root:FindFirstChild("FlyBG") then root.FlyBG:Destroy() end
    end
end

function ViewPlayer()
    local name = game:GetService("StarterGui"):PromptInput("Nome do jogador para View:")
    local target = Players:FindFirstChild(name)
    if target and target.Character and target.Character:FindFirstChild("Head") then
        workspace.CurrentCamera.CameraSubject = target.Character.Head
    end
end

function UnView()
    if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Head") then
        workspace.CurrentCamera.CameraSubject = LocalPlayer.Character.Head
    end
end

function FlingAll()
    for _,plr in pairs(Players:GetPlayers()) do
        if plr ~= LocalPlayer and plr.Character and plr.Character:FindFirstChild("HumanoidRootPart") then
            plr.Character.HumanoidRootPart.Velocity = Vector3.new(500,500,500)
        end
    end
end

function UnFling()
    for _,plr in pairs(Players:GetPlayers()) do
        if plr ~= LocalPlayer and plr.Character and plr.Character:FindFirstChild("HumanoidRootPart") then
            plr.Character.HumanoidRootPart.Velocity = Vector3.new(0,0,0)
        end
    end
end

function PetDoJogador()
    local name = game:GetService("StarterGui"):PromptInput("Nome do jogador para virar Pet:")
    local target = Players:FindFirstChild(name)
    if target and target.Character and LocalPlayer.Character then
        LocalPlayer.Character.HumanoidRootPart.CFrame = target.Character.HumanoidRootPart.CFrame * CFrame.new(2,0,0)
        local weld = Instance.new("WeldConstraint", LocalPlayer.Character.HumanoidRootPart)
        weld.Part0 = LocalPlayer.Character.HumanoidRootPart
        weld.Part1 = target.Character.HumanoidRootPart
        weld.Name = "PetWeld"
    end
end

function UnPet()
    if LocalPlayer.Character and LocalPlayer.Character.HumanoidRootPart:FindFirstChild("PetWeld") then
        LocalPlayer.Character.HumanoidRootPart.PetWeld:Destroy()
    end
end

-- Botões
local y = 180
CreateButton("ESP", y, EnableESP)
y = y + 40
CreateButton("UnESP", y, DisableESP)
y = y + 40
CreateButton("Fly", y, Fly)
y = y + 40
CreateButton("UnFly", y, UnFly)
y = y + 40
CreateButton("View Jogador", y, ViewPlayer)
y = y + 40
CreateButton("UnView", y, UnView)
y = y + 40
CreateButton("Fling All", y, FlingAll)
y = y + 40
CreateButton("UnFling", y, UnFling)
y = y + 40
CreateButton("Vira Pet do Jogador", y, PetDoJogador)
y = y + 40
CreateButton("Desvirar Pet", y, UnPet)

-- Resize Panel
local ResizeBtn = Instance.new("TextButton")
ResizeBtn.Text = "⬍"
ResizeBtn.Size = UDim2.new(0, 36, 0, 36)
ResizeBtn.Position = UDim2.new(1, -40, 1, -40)
ResizeBtn.BackgroundColor3 = Color3.fromRGB(40,40,40)
ResizeBtn.TextColor3 = Color3.fromRGB(255,255,255)
ResizeBtn.Font = Enum.Font.GothamBold
ResizeBtn.TextSize = 22
ResizeBtn.Parent = Panel

local min = false
ResizeBtn.MouseButton1Click:Connect(function()
    if not min then
        Panel.Size = UDim2.new(0, 180, 0, 60)
        ResizeBtn.Position = UDim2.new(1, -40, 1, -40)
        min = true
    else
        Panel.Size = UDim2.new(0, 350, 0, 450)
        ResizeBtn.Position = UDim2.new(1, -40, 1, -40)
        min = false
    end
end)
