local frame = script.Parent
local openCloseButton = Instance.new("TextButton")
openCloseButton.Parent = frame.Parent
openCloseButton.Size = UDim2.new(0, 100, 0, 30)
openCloseButton.Position = UDim2.new(0, 10, 0, 10)
openCloseButton.Text = "Abrir Painel"

frame.Visible = false

-- Botão de abrir/fechar painel
openCloseButton.MouseButton1Click:Connect(function()
    frame.Visible = not frame.Visible
    openCloseButton.Text = frame.Visible and "Fechar Painel" or "Abrir Painel"
end)

-- Botão 1: Copiar método do jogador selecionado (exemplo)
local copyButton = Instance.new("TextButton")
copyButton.Parent = frame
copyButton.Size = UDim2.new(0, 200, 0, 30)
copyButton.Position = UDim2.new(0, 10, 0, 10)
copyButton.Text = "Copiar método do jogador selecionado"
copyButton.MouseButton1Click:Connect(function()
    -- Exemplo: copiar o nome do jogador selecionado
    local selectedPlayer = game.Players:GetPlayers()[1] -- Troque por seu método de seleção
    if selectedPlayer then
        setclipboard(selectedPlayer.Name)
    end
end)

-- Botão 2: ESP na cabeça
local espButton = Instance.new("TextButton")
espButton.Parent = frame
espButton.Size = UDim2.new(0, 200, 0, 30)
espButton.Position = UDim2.new(0, 10, 0, 50)
espButton.Text = "ESP na Cabeça"
espButton.MouseButton1Click:Connect(function()
    for i, player in pairs(game.Players:GetPlayers()) do
        if player.Character and player.Character:FindFirstChild("Head") then
            local billboard = Instance.new("BillboardGui", player.Character.Head)
            billboard.Size = UDim2.new(0, 50, 0, 50)
            billboard.Adornee = player.Character.Head
            billboard.AlwaysOnTop = true

            local label = Instance.new("TextLabel", billboard)
            label.Size = UDim2.new(1, 0, 1, 0)
            label.Text = player.Name
            label.BackgroundTransparency = 1
            label.TextColor3 = Color3.new(1, 0, 0)
        end
    end
end)

-- Botão 3: UnESP (remover ESP)
local unespButton = Instance.new("TextButton")
unespButton.Parent = frame
unespButton.Size = UDim2.new(0, 200, 0, 30)
unespButton.Position = UDim2.new(0, 10, 0, 90)
unespButton.Text = "UnESP"
unespButton.MouseButton1Click:Connect(function()
    for i, player in pairs(game.Players:GetPlayers()) do
        if player.Character and player.Character:FindFirstChild("Head") then
            for _, gui in pairs(player.Character.Head:GetChildren()) do
                if gui:IsA("BillboardGui") then
                    gui:Destroy()
                end
            end
        end
    end
end)
