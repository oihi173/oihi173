local Players = game:GetService("Players")
local player = Players.LocalPlayer
local mouse = player:GetMouse()

-- Criar GUI
local ScreenGui = script.Parent
local Frame = Instance.new("Frame", ScreenGui)
Frame.Size = UDim2.new(0, 250, 0, 300)
Frame.Position = UDim2.new(0, 50, 0, 100)
Frame.BackgroundColor3 = Color3.fromRGB(40,40,40)
Frame.Active = true
Frame.Draggable = true -- permite arrastar
Frame.Visible = true

local UICorner = Instance.new("UICorner", Frame)
UICorner.CornerRadius = UDim.new(0,10)

-- Botão abrir/fechar
local ToggleButton = Instance.new("TextButton", ScreenGui)
ToggleButton.Size = UDim2.new(0,100,0,40)
ToggleButton.Position = UDim2.new(0,50,0,60)
ToggleButton.Text = "Abrir/Fechar Painel ADM"
ToggleButton.BackgroundColor3 = Color3.fromRGB(70,70,70)

ToggleButton.MouseButton1Click:Connect(function()
	Frame.Visible = not Frame.Visible
end)

-- Função para criar botões
local function createButton(name, posY)
	local btn = Instance.new(
 local dir = directions[math.random(1,#directions)]
                char.Humanoid:Move(dir, true)
            end
        end)
    else
        if iaConn then iaConn:Disconnect() end
        btnIA.Text = "IA Movement"
    end
end)
