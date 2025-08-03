local Player = game.Players.LocalPlayer
local PlayerGui = Player:WaitForChild("PlayerGui")

-- Criação do ScreenGui e Frame
local gui = Instance.new("ScreenGui")
gui.Name = "MovableYellowPanel"
gui.Parent = PlayerGui

local panel = Instance.new("Frame")
panel.Size = UDim2.new(0, 300, 0, 260)
panel.Position = UDim2.new(0.1, 0, 0.1, 0)
panel.BackgroundColor3 = Color3.fromRGB(255, 255, 0) -- Amarelo
panel.BorderSizePixel = 2
panel.Parent = gui
panel.Active = true
panel.Draggable = true -- permite mover

-- Título
local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, 0, 0, 30)
title.BackgroundTransparency = 1
title.Text = "Painel Completo Roblox"
title.Font = Enum.Font.SourceSansBold
title.TextSize = 20
title.TextColor3 = Color3.new(0,0,0)
title.Parent = panel

-- Botão de Fechar/Abrir
local closeBtn = Instance.new("TextButton")
closeBtn.Size = UDim2.new(0, 30, 0, 30)
closeBtn.Position = UDim2.new(1, -35, 0, 5)
closeBtn.Text = "X"
closeBtn.TextColor3 = Color3.fromRGB(255,0,0)
closeBtn.Parent = panel

-- Botão Painel Completo (Mostrar/Ocultar)
local fullBtn = Instance.new("TextButton")
fullBtn.Size = UDim2.new(0, 130, 0, 25)
fullBtn.Position = UDim2.new(0.5, -65, 1, -35)
fullBtn.Text = "Painel Completo: ON"
fullBtn.Parent = panel

-- ESP/UNESP Switch
local esp = false
local espBtn = Instance.new("TextButton")
espBtn.Size = UDim2.new(0,120,0,35)
espBtn.Position = UDim2.new(0,10,0,50)
espBtn.Text = "ESP: OFF"
espBtn.BackgroundColor3 = Color3.fromRGB(220,220,220)
espBtn.TextColor3 = Color3.new(0,0,0)
espBtn.Parent = panel

local unesp = false
local unespBtn = Instance.new("TextButton")
unespBtn.Size = UDim2.new(0,120,0,35)
unespBtn.Position = UDim2.new(0,10,0,100)
unespBtn.Text = "UNESP: OFF"
unespBtn.BackgroundColor3 = Color3.fromRGB(220,220,220)
unespBtn.TextColor3 = Color3.new(0,0,0)
unespBtn.Parent = panel

-- View/Unview (ESP)
local viewBtn = Instance.new("TextButton")
viewBtn.Size = UDim2.new(0,120,0,35)
viewBtn.Position = UDim2.new(0,160,0,50)
viewBtn.Text = "View ESP"
viewBtn.BackgroundColor3 = Color3.fromRGB(180,180,255)
viewBtn.TextColor3 = Color3.new(0,0,0)
viewBtn.Parent = panel

local unviewBtn = Instance.new("TextButton")
unviewBtn.Size = UDim2.new(0,120,0,35)
unviewBtn.Position = UDim2.new(0,160,0,100)
unviewBtn.Text = "Unview ESP"
unviewBtn.BackgroundColor3 = Color3.fromRGB(180,180,255)
unviewBtn.TextColor3 = Color3.new(0,0,0)
unviewBtn.Parent = panel

-- Abraço AL jogando mais próximo
local abracoBtn = Instance.new("TextButton")
abracoBtn.Size = UDim2.new(0,260,0,40)
abracoBtn.Position = UDim2.new(0,20,0,160)
abracoBtn.Text = "Abraço AL Jogando Mais Próximo"
abracoBtn.BackgroundColor3 = Color3.fromRGB(255,200,0)
abracoBtn.TextColor3 = Color3.fromRGB(0,0,0)
abracoBtn.Font = Enum.Font.SourceSansBold
abracoBtn.TextSize = 16
abracoBtn.Parent = panel

-- Lógica dos botões

closeBtn.MouseButton1Click:Connect(function()
    panel.Visible = false
    -- Cria um botão para reabrir painel
    if not gui:FindFirstChild("openBtn") then
        local openBtn = Instance.new("TextButton")
        openBtn.Name = "openBtn"
        openBtn.Size = UDim2.new(0,80,0,30)
        openBtn.Position = UDim2.new(0.1, 0, 0.1, 270)
        openBtn.Text = "Abrir Painel"
        openBtn.Parent = gui
        openBtn.MouseButton1Click:Connect(function()
            panel.Visible = true
            openBtn:Destroy()
        end)
    end
end)

fullBtn.MouseButton1Click:Connect(function()
    panel.Visible = not panel.Visible
    fullBtn.Text = panel.Visible and "Painel Completo: ON" or "Painel Completo: OFF"
end)

espBtn.MouseButton1Click:Connect(function()
    esp = not esp
    espBtn.Text = esp and "ESP: ON" or "ESP: OFF"
    espBtn.BackgroundColor3 = esp and Color3.fromRGB(100,255,100) or Color3.fromRGB(220,220,220)
    -- Adicione sua lógica de ESP aqui!
end)

unespBtn.MouseButton1Click:Connect(function()
    unesp = not unesp
    unespBtn.Text = unesp and "UNESP: ON" or "UNESP: OFF"
    unespBtn.BackgroundColor3 = unesp and Color3.fromRGB(255,100,100) or Color3.fromRGB(220,220,220)
    -- Adicione sua lógica de UNESP aqui!
end)

viewBtn.MouseButton1Click:Connect(function()
    -- Ativa ESP
    esp = true
    espBtn.Text = "ESP: ON"
    espBtn.BackgroundColor3 = Color3.fromRGB(100,255,100)
    -- Lógica de view
end)

unviewBtn.MouseButton1Click:Connect(function()
    -- Desativa ESP
    esp = false
    espBtn.Text = "ESP: OFF"
    espBtn.BackgroundColor3 = Color3.fromRGB(220,220,220)
    -- Lógica de unview
end)

abracoBtn.MouseButton1Click:Connect(function()
    -- Lógica: Jogar próximo ao jogador mais perto (exemplo simples)
    local char = Player.Character
    if not char then return end
    local humanoidRoot = char:FindFirstChild("HumanoidRootPart")
    if not humanoidRoot then return end

    local closestDist = math.huge
    local closestPlayer = nil

    for _,plr in pairs(game.Players:GetPlayers()) do
        if plr ~= Player and plr.Character and plr.Character:FindFirstChild("HumanoidRootPart") then
            local dist = (plr.Character.HumanoidRootPart.Position - humanoidRoot.Position).magnitude
            if dist < closestDist then
                closestDist = dist
                closestPlayer = plr
            end
        end
    end

    if closestPlayer and closestPlayer.Character and closestPlayer.Character:FindFirstChild("HumanoidRootPart") then
        humanoidRoot.CFrame = closestPlayer.Character.HumanoidRootPart.CFrame * CFrame.new(2,0,0)
    end
end)
