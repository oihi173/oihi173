local Players = game:GetService("Players")
local player = Players.LocalPlayer

-- Painel UI
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "DeadRailsESP"
screenGui.Parent = player:WaitForChild("PlayerGui")

local panel = Instance.new("Frame")
panel.Size = UDim2.new(0, 250, 0, 120)
panel.Position = UDim2.new(0, 50, 0, 50)
panel.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
panel.BorderColor3 = Color3.fromRGB(255, 221, 0)
panel.BorderSizePixel = 2
panel.Active = true
panel.Draggable = true -- Mover painel
panel.Parent = screenGui

local header = Instance.new("TextLabel")
header.Size = UDim2.new(1, 0, 0, 30)
header.BackgroundTransparency = 1
header.Text = "Painel ESP Dead Rails"
header.TextColor3 = Color3.fromRGB(255, 221, 0)
header.Font = Enum.Font.SourceSansBold
header.TextSize = 20
header.Parent = panel

local closeBtn = Instance.new("TextButton")
closeBtn.Size = UDim2.new(0, 30, 0, 30)
closeBtn.Position = UDim2.new(1, -35, 0, 0)
closeBtn.BackgroundColor3 = Color3.fromRGB(40,40,40)
closeBtn.Text = "X"
closeBtn.TextColor3 = Color3.fromRGB(255, 221, 0)
closeBtn.Font = Enum.Font.SourceSansBold
closeBtn.TextSize = 20
closeBtn.Parent = panel

local openBtn = Instance.new("TextButton")
openBtn.Size = UDim2.new(0, 120, 0, 30)
openBtn.Position = UDim2.new(0, 50, 0, 10)
openBtn.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
openBtn.Text = "Abrir Painel ESP"
openBtn.TextColor3 = Color3.fromRGB(255, 221, 0)
openBtn.Font = Enum.Font.SourceSansBold
openBtn.TextSize = 16
openBtn.Visible = false
openBtn.Parent = screenGui

closeBtn.MouseButton1Click:Connect(function()
    panel.Visible = false
    openBtn.Visible = true
end)

openBtn.MouseButton1Click:Connect(function()
    panel.Visible = true
    openBtn.Visible = false
end)

local espBtn = Instance.new("TextButton")
espBtn.Size = UDim2.new(0, 120, 0, 30)
espBtn.Position = UDim2.new(0, 10, 0, 40)
espBtn.BackgroundColor3 = Color3.fromRGB(40,40,40)
espBtn.Text = "Ativar ESP"
espBtn.TextColor3 = Color3.fromRGB(255, 221, 0)
espBtn.Font = Enum.Font.SourceSansBold
espBtn.TextSize = 16
espBtn.Parent = panel

local unespBtn = Instance.new("TextButton")
unespBtn.Size = UDim2.new(0, 80, 0, 30)
unespBtn.Position = UDim2.new(0, 140, 0, 40)
unespBtn.BackgroundColor3 = Color3.fromRGB(40,40,40)
unespBtn.Text = "UnESP"
unespBtn.TextColor3 = Color3.fromRGB(255, 221, 0)
unespBtn.Font = Enum.Font.SourceSansBold
unespBtn.TextSize = 16
unespBtn.Parent = panel

local info = Instance.new("TextLabel")
info.Size = UDim2.new(1, -20, 0, 30)
info.Position = UDim2.new(0, 10, 0, 80)
info.BackgroundTransparency = 1
info.Text = "ESP: Só linha amarela na hitbox dos zumbis"
info.TextColor3 = Color3.fromRGB(255, 221, 0)
info.Font = Enum.Font.SourceSans
info.TextSize = 13
info.Parent = panel

-- Função ESP
local adornments = {}
local espActive = false

function clearESP()
    for _, ad in ipairs(adornments) do
        ad:Destroy()
    end
    adornments = {}
end

function updateESP()
    clearESP()
    if not espActive then return end
    local zombiesFolder = workspace:FindFirstChild("Zombies")
    if not zombiesFolder then return end

    for _, zombie in ipairs(zombiesFolder:GetChildren()) do
        if zombie:IsA("Model") then
            local part = zombie:FindFirstChild("HumanoidRootPart") or zombie:FindFirstChildWhichIsA("BasePart")
            if part then
                local box = Instance.new("BoxHandleAdornment")
                box.Size = part.Size
                box.Adornee = part
                box.Parent = part
                box.Color3 = Color3.fromRGB(255, 221, 0)
                box.Transparency = 0
                box.ZIndex = 10
                box.AlwaysOnTop = true
                box.Visible = true
                box.LineThickness = 2
                box.Name = "DeadRailsESPBox"
                box.Style = Enum.BoxHandleAdornmentStyle.Wireframe
                table.insert(adornments, box)
            end
        end
    end
end

espBtn.MouseButton1Click:Connect(function()
    espActive = not espActive
    espBtn.Text = espActive and "Desativar ESP" or "Ativar ESP"
    updateESP()
end)

unespBtn.MouseButton1Click:Connect(function()
    espActive = false
    espBtn.Text = "Ativar ESP"
    clearESP()
end)

-- Atualiza ESP quando zumbis aparecerem/desaparecerem
workspace.ChildAdded:Connect(function(child)
    if child.Name == "Zombies" then
        updateESP()
        child.ChildAdded:Connect(function() updateESP() end)
        child.ChildRemoved:Connect(function() updateESP() end)
    end
end)

local zombiesFolder = workspace:FindFirstChild("Zombies")
if zombiesFolder then
    zombiesFolder.ChildAdded:Connect(function() updateESP() end)
    zombiesFolder.ChildRemoved:Connect(function() updateESP() end)
end

-- Para garantir que sempre limpa ao fechar painel
panel:GetPropertyChangedSignal("Visible"):Connect(function()
    if not panel.Visible then
        clearESP()
    end
end)
