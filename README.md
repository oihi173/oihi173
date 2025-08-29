-- Wild Horse Islands Script - Informativo (GUI)
local Players = game:GetService("Players")
local player = Players.LocalPlayer

-- Cria a tela GUI principal
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "WildHorseInfo"
screenGui.Parent = player:WaitForChild("PlayerGui")

-- Janela principal
local frame = Instance.new("Frame")
frame.Size = UDim2.new(0.4, 0, 0.6, 0)
frame.Position = UDim2.new(0.3, 0, 0.2, 0)
frame.BackgroundColor3 = Color3.fromRGB(32, 32, 32)
frame.BorderSizePixel = 0
frame.Parent = screenGui

-- Título
local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, 0, 0.1, 0)
title.Position = UDim2.new(0, 0, 0, 0)
title.BackgroundTransparency = 1
title.Text = "Wild Horse Islands Script 2025"
title.Font = Enum.Font.GothamBold
title.TextSize = 28
title.TextColor3 = Color3.fromRGB(255, 200, 64)
title.Parent = frame

-- Texto informativo
local info = Instance.new("TextLabel")
info.Size = UDim2.new(1, -16, 0.85, -16)
info.Position = UDim2.new(0, 8, 0.12, 8)
info.BackgroundTransparency = 1
info.TextWrapped = true
info.TextYAlignment = Enum.TextYAlignment.Top
info.Text = [[
✨ Auto Farm: Coleta maçãs, flores, minérios, doma cavalos e completa missões automaticamente!
🔄 Teleporte: Viaje instantaneamente entre ilhas sem animação.
✨ Fly: Voe pelo mapa para encontrar cavalos raros ou escapar de terrenos difíceis.
💰 Auto Sell & Collect: Recursos coletados e vendidos automaticamente.
📈 GUI otimizada: Ative/desative funções facilmente.
🔗 Pastebin Fetch: Atualizações automáticas via Pastebin.
🌟 Compatível com PC e Mobile!

Dicas:
- Use auto farm durante eventos.
- Ative fly para coleta em regiões altas.
- Use em servidores privados para menos risco.

Requisitos:
- Windows 10+, macOS 12+, iOS 15+, Android 10+
- 4GB RAM, processador 1.6GHz+

Para instalar: Use um executor confiável (Synapse X, Fluxus, Arceus X, etc.) e execute o script!

Aproveite e compartilhe com a comunidade!
]]
info.Font = Enum.Font.Gotham
info.TextSize = 16
info.TextColor3 = Color3.fromRGB(240, 240, 240)
info.Parent = frame

-- Botão de fechar
local closeBtn = Instance.new("TextButton")
closeBtn.Size = UDim2.new(0.15, 0, 0.08, 0)
closeBtn.Position = UDim2.new(0.82, 0, 0, 8)
closeBtn.BackgroundColor3 = Color3.fromRGB(200, 60, 60)
closeBtn.Text = "Fechar"
closeBtn.Font = Enum.Font.GothamSemibold
closeBtn.TextSize = 16
closeBtn.TextColor3 = Color3.new(1,1,1)
closeBtn.Parent = frame

closeBtn.MouseButton1Click:Connect(function()
	screenGui:Destroy()
end)
