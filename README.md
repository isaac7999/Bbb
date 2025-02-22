-- Criando a janela principal do Hub
local HubWindow = Instance.new("Frame")
HubWindow.Size = UDim2.new(0, 900, 0, 600)  -- Largura maior para o menu
HubWindow.Position = UDim2.new(0.5, -450, 0.5, -300)
HubWindow.BackgroundColor3 = Color3.fromRGB(35, 35, 35)  -- Fundo escuro (preto claro)
HubWindow.BackgroundTransparency = 0.8
HubWindow.BorderSizePixel = 0
HubWindow.Parent = ScreenGui
HubWindow.ClipsDescendants = true

-- Adicionando um título à janela
local titleLabel = Instance.new("TextLabel")
titleLabel.Size = UDim2.new(1, 0, 0, 60)
titleLabel.Text = "AWAYS HUB"
titleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
titleLabel.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
titleLabel.TextSize = 30
titleLabel.Font = Enum.Font.GothamBold
titleLabel.TextXAlignment = Enum.TextXAlignment.Center
titleLabel.Parent = HubWindow

-- Variáveis para armazenar o estado do Noclip
titleLabel.Parent = HubWindow
local noclipEnabled = false
local godmodeEnabled = false
local humanoid = game.Players.LocalPlayer.Character:WaitForChild("Humanoid")
local character = game.Players.LocalPlayer.Character

-- Função para ativar/desativar Noclip
local function ToggleNoclip()
    noclipEnabled = not noclipEnabled
    if noclipEnabled then
        for _, part in pairs(character:GetChildren()) do
            if part:IsA("BasePart") then
                part.CanCollide = false
            end
        end
        print("Noclip ativado!")
    else
        for _, part in pairs(character:GetChildren()) do
            if part:IsA("BasePart") then
                part.CanCollide = true
            end
        end
        print("Noclip desativado!")
    end
end

-- Função para ativar/desativar Godmode
local function ToggleGodmode()
    godmodeEnabled = not godmodeEnabled
    if godmodeEnabled then
        humanoid.HealthChanged:Connect(function()
            if humanoid.Health < humanoid.MaxHealth then
                humanoid.Health = humanoid.MaxHealth
            end
        end)
        print("Godmode ativado!")
    else
        humanoid.HealthChanged:Disconnect()
        print("Godmode desativado!")
    end
end

-- Função para ativar/desativar ESP Nick
local function ToggleEspNick()
    for _, player in pairs(game.Players:GetPlayers()) do
        if player ~= game.Players.LocalPlayer then
            local billboard = Instance.new("BillboardGui", player.Character.Head)
            billboard.Size = UDim2.new(0, 200, 0, 50)
            billboard.Adornee = player.Character.Head
            billboard.StudsOffset = Vector3.new(0, 2, 0)
            
            local textLabel = Instance.new("TextLabel", billboard)
            textLabel.Size = UDim2.new(1, 0, 1, 0)
            textLabel.Text = player.Name
            textLabel.TextColor3 = Color3.fromRGB(0, 0, 139)
            textLabel.BackgroundTransparency = 1
            textLabel.TextSize = 14
            textLabel.Font = Enum.Font.GothamBold
        end
    end
end

-- Função para ativar/desativar ESP Line
local function ToggleEspLine()
    for _, player in pairs(game.Players:GetPlayers()) do
        if player ~= game.Players.LocalPlayer then
            local line = Instance.new("Beam")
            line.Parent = game.Workspace
            line.Attachment0 = Instance.new("Attachment", game.Players.LocalPlayer.Character.Head)
            line.Attachment1 = Instance.new("Attachment", player.Character.Head)
            line.Color = ColorSequence.new(Color3.fromRGB(255, 0, 0))
            line.Width0 = 0.1
            line.Width1 = 0.1
        end
    end
end

-- Criando botões no painel
CreateButton("Rejoin", "Rejoin the game", LeftPanel, UDim2.new(0, 10, 0, 70), function()
    local ts = game:GetService("TeleportService")
    local p = game:GetService("Players").LocalPlayer
    ts:Teleport(game.PlaceId, p)
    print("Rejoin executado!")
end)

CreateButton("Noclip", "Ativar/desativar Noclip", LeftPanel, UDim2.new(0, 10, 0, 140), function()
    ToggleNoclip()
end)

CreateButton("Godmode", "Ativar/desativar Godmode", LeftPanel, UDim2.new(0, 10, 0, 210), function()
    ToggleGodmode()
end)

CreateButton("ESP Nick", "Exibir nomes dos jogadores", LeftPanel, UDim2.new(0, 10, 0, 280), function()
    ToggleEspNick()
end)

CreateButton("ESP Line", "Conectar jogadores com linha", LeftPanel, UDim2.new(0, 10, 0, 350), function()
    ToggleEspLine()
end)
