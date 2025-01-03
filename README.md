-- Função para criar a caixa ESP com bordas brancas
local function createESP(player)
    local Box = Instance.new("BoxHandleAdornment")
    Box.Size = Vector3.new(4, 6, 1)
    Box.Name = "ESPBox"
    Box.Adornee = player.Character and player.Character:FindFirstChild("HumanoidRootPart")
    Box.Color3 = Color3.fromRGB(255, 255, 255) -- Cor da borda (branco)
    Box.AlwaysOnTop = true
    Box.ZIndex = 1
    Box.Transparency = 0 -- Bordas totalmente opacas
    Box.AdornCullingMode = Enum.AdornCullingMode.Never
    Box.Parent = game.Workspace
end

-- Adiciona ESP a todos os jogadores existentes
local function addESPToAllPlayers()
    for _, player in pairs(game.Players:GetPlayers()) do
        if player ~= game.Players.LocalPlayer then
            createESP(player)
        end
    end
end

-- Atualiza o ESP conforme novos jogadores entram
game.Players.PlayerAdded:Connect(function(player)
    player.CharacterAdded:Connect(function()
        wait(1) -- Espera um momento para garantir que o personagem foi carregado
        createESP(player)
    end)
end)

-- Remove ESP quando jogadores saem
game.Players.PlayerRemoving:Connect(function(player)
    if player.Character and player.Character:FindFirstChild("ESPBox") then
        player.Character.ESPBox:Destroy()
    end
end)

-- Inicializa o script
addESPToAllPlayers()
