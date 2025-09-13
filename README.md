LEIA-ME.md
-- Cria a interface do menu local ScreenGui = Instance.new("ScreenGui") ScreenGui.Name = "AimbotESPMenu" ScreenGui.Parent = game.Players.LocalPlayer:WaitForChild("PlayerGui")

Quadro local = Instância.new("Quadro") Tamanho do Quadro = UDim2.new(0, 250, 0, 150) Posição do Quadro = UDim2.new(0, 10, 0, 10) Cor do Fundo3 = Cor3.fromRGB(30, 30, 30) Tamanho da Borda do Quadro = 0 Quadro Pai = ScreenGui

UIListLayout local = Instance.new("UIListLayout") UIListLayout.Padding = UDim.new(0, 5) UIListLayout.FillDirection = Enum.FillDirection.Vertical UIListLayout.Parent = Frame

-- Função para criar botões local function createButton(text) local button = Instance.new("TextButton") button.Size = UDim2.new(1, -10, 0, 30) button.BackgroundColor3 = Color3.fromRGB(50, 50, 50) button.TextColor3 = Color3.new(1, 1, 1) button.Text = texto button.Font = Enum.Font.SourceSans button.TextSize = 18 button.Parent = Frame return button end

-- Função para criar rótulos local function createLabel(texto) local label = Instance.new("TextLabel") label.Size = UDim2.new(1, -10, 0, 20) label.BackgroundTransparency = 1 label.TextColor3 = Color3.new(1, 1, 1) label.Text = texto label.Font = Enum.Font.SourceSans label.TextSize = 16 label.TextXAlignment = Enum.TextXAlignment.Left label.Parent = Frame return label end

-- Função para criar sliders local function createSlider(min, max, default) local sliderFrame = Instance.new("Frame") sliderFrame.Size = UDim2.new(1, -10, 0, 30) sliderFrame.BackgroundColor3 = Color3.fromRGB(50, 50, 50) sliderFrame.Parent = Frame

local sliderBar = Instance.new("Frame")
sliderBar.Size = UDim2.new(1, -40, 0, 10)
sliderBar.Position = UDim2.new(0, 10, 0, 10)
sliderBar.BackgroundColor3 = Color3.fromRGB(100, 100, 100)
sliderBar.Parent = sliderFrame

local sliderFill = Instance.new("Frame")
sliderFill.Size = UDim2.new((default - min) / (max - min), 0, 1, 0)
sliderFill.BackgroundColor3 = Color3.fromRGB(0, 170, 255)
sliderFill.Parent = sliderBar

local sliderButton = Instance.new("TextButton")
sliderButton.Size = UDim2.new(0, 20, 0, 20)
sliderButton.Position = UDim2.new(sliderFill.Size.X.Scale, 0, 0, 0)
sliderButton.BackgroundColor3 = Color3.fromRGB(0, 120, 200)
sliderButton.Text = ""
sliderButton.Parent = sliderBar

local valueLabel = Instance.new("TextLabel")
valueLabel.Size = UDim2.new(0, 30, 0, 20)
valueLabel.Position = UDim2.new(1, 5, 0, 5)
valueLabel.BackgroundTransparency = 1
valueLabel.TextColor3 = Color3.new(1, 1, 1)
valueLabel.Text = tostring(default)
valueLabel.Font = Enum.Font.SourceSans
valueLabel.TextSize = 16
valueLabel.Parent = sliderFrame

local dragging = false

sliderButton.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = true
    end
end)

sliderButton.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = false
    end
end)

sliderBar.InputChanged:Connect(function(input)
    if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
        local relativeX = math.clamp(input.Position.X - sliderBar.AbsolutePosition.X, 0, sliderBar.AbsoluteSize.X)
        local scale = relativeX / sliderBar.AbsoluteSize.X
        sliderFill.Size = UDim2.new(scale, 0, 1, 0)
        sliderButton.Position = UDim2.new(scale, 0, 0, 0)
        local value = math.floor(min + (max - min) * scale)
        valueLabel.Text = tostring(value)
        sliderFrame.Value = value
    end
end)

sliderFrame.Value = default
return sliderFrame
fim

-- Variáveis ​​de controle local espEnabled = false local aimbotEnabled = false local aimbotDistance = 100

-- Botões local espButton = createButton("ESP: OFF") local aimbotButton = createButton("Aimbot: OFF") local distanceLabel = createLabel("Distância do Aimbot:") local distanceSlider = createSlider(10, 500, aimbotDistance)

-- Atualiza o estado do ESP espButton.MouseButton1Click:Connect(function() espEnabled = not espEnabled espButton.Text = "ESP: " .. (espEnabled and "ON" or "OFF") -- Aqui você pode ativar/desativar seu código ESP print("ESP ativado:", espEnabled) end)

-- Atualiza o estado do Aimbot aimbotButton.MouseButton1Click:Connect(function() aimbotEnabled = not aimbotEnabled aimbotButton.Text = "Aimbot: " .. (aimbotEnabled and "ON" or "OFF") -- Aqui você pode ativar/desativar seu código Aimbot print("Aimbot ativado:", aimbotEnabled) end)

-- Atualiza a distância do aimbot distanceSlider:GetPropertyChangedSignal("Value"):Connect(function() aimbotDistance = distanceSlider.Value print("Distância do Aimbot:", aimbotDistance) end)

-- Exemplo simples de ESP (destacar jogadores) local function toggleESP(state) for _, player in pairs(game.Players:GetPlayers()) do if player ~= game.Players.LocalPlayer and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then local highlight = player.Character:FindFirstChild("Highlight") if state then if not highlight then highlight = Instance.new("Highlight") highlight.Name = "Highlight" highlight.Adornee = player.Character highlight.Parent = player.Character end else if highlight then highlight:Destroy() end end end end end

-- Atualiza ESP quando ativado/desativado espButton.MouseButton1Click:Connect(function() toggleESP(espEnabled) end)

-- Exemplo simples de aimbot (apontar para o jogador mais próximo dentro da distância) local RunService = game:GetService("RunService") local camera = workspace.CurrentCamera

RunService.RenderStepped:Connect(function() se aimbotEnabled então local closestPlayer = nil local closestDistance = aimbotDistance + 1 local localPlayer = game.Players.LocalPlayer local localCharacter = localPlayer.Character se não for localCharacter ou não for localCharacter:FindFirstChild("HumanoidRootPart") então retorne final local localPos = localCharacter.HumanoidRootPart.Position

    for _, player in pairs(game.Players:GetPlayers()) do
        if player ~= localPlayer and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
            local targetPos = player.Character.HumanoidRootPart.Position
            local dist = (targetPos - localPos).Magnitude
            if dist < closestDistance then
                closestDistance = dist
                closestPlayer = player
            end
        end
    end

    if closestPlayer and closestPlayer.Character and closestPlayer.Character:FindFirstChild("HumanoidRootPart") then
        local targetPos = closestPlayer.Character.HumanoidRootPart.Position
        camera.CFrame = CFrame.new(camera.CFrame.Position, targetPos)
    end
end
fim)
