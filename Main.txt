-- Script de Ping/FPS Monitor Móvel para Roblox
-- Compatível com Mobile (touch) e PC (mouse)

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local Stats = game:GetService("Stats")

local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

-- Criação da GUI principal
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "PingFPSMonitor"
screenGui.ResetOnSpawn = false
screenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
screenGui.Parent = playerGui

-- Frame principal
local mainFrame = Instance.new("Frame")
mainFrame.Name = "MainFrame"
mainFrame.Size = UDim2.new(0, 120, 0, 50)
mainFrame.Position = UDim2.new(0, 10, 0, 10) -- Canto superior esquerdo
mainFrame.BackgroundColor3 = Color3.fromRGB(75, 0, 130) -- Roxo escuro
mainFrame.BorderSizePixel = 0
mainFrame.Active = true
mainFrame.Draggable = true -- Ativa draggable nativo do Roblox
mainFrame.Parent = screenGui

-- Cantos arredondados
local corner = Instance.new("UICorner")
corner.CornerRadius = UDim.new(0, 6)
corner.Parent = mainFrame

-- Gradiente sutil
local gradient = Instance.new("UIGradient")
gradient.Color = ColorSequence.new{
    ColorSequenceKeypoint.new(0, Color3.fromRGB(85, 10, 140)),
    ColorSequenceKeypoint.new(1, Color3.fromRGB(65, 0, 120))
}
gradient.Rotation = 45
gradient.Parent = mainFrame

-- Label do Ping
local pingLabel = Instance.new("TextLabel")
pingLabel.Name = "PingLabel"
pingLabel.Size = UDim2.new(1, 0, 0.5, 0)
pingLabel.Position = UDim2.new(0, 0, 0, 0)
pingLabel.BackgroundTransparency = 1
pingLabel.Text = "Ping: --"
pingLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
pingLabel.TextScaled = true
pingLabel.TextStrokeTransparency = 0
pingLabel.TextStrokeColor3 = Color3.fromRGB(0, 0, 0)
pingLabel.Font = Enum.Font.Gotham
pingLabel.Parent = mainFrame

-- Label do FPS
local fpsLabel = Instance.new("TextLabel")
fpsLabel.Name = "FpsLabel"
fpsLabel.Size = UDim2.new(1, 0, 0.5, 0)
fpsLabel.Position = UDim2.new(0, 0, 0.5, 0)
fpsLabel.BackgroundTransparency = 1
fpsLabel.Text = "FPS: --"
fpsLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
fpsLabel.TextScaled = true
fpsLabel.TextStrokeTransparency = 0
fpsLabel.TextStrokeColor3 = Color3.fromRGB(0, 0, 0)
fpsLabel.Font = Enum.Font.Gotham
fpsLabel.Parent = mainFrame

-- Variáveis para cálculo de FPS
local frameCount = 0
local lastTime = tick()
local fps = 0

-- Função para atualizar o ping
local function updatePing()
    local ping = player:GetNetworkPing() * 1000 -- Converte para ms
    local roundedPing = math.floor(ping + 0.5)
    
    -- Cor baseada no ping
    local pingColor = Color3.fromRGB(0, 255, 0) -- Verde (bom)
    if roundedPing > 100 then
        pingColor = Color3.fromRGB(255, 255, 0) -- Amarelo (médio)
    end
    if roundedPing > 200 then
        pingColor = Color3.fromRGB(255, 0, 0) -- Vermelho (ruim)
    end
    
    pingLabel.Text = "Ping: " .. roundedPing
    pingLabel.TextColor3 = pingColor
end

-- Função para atualizar o FPS
local function updateFPS()
    frameCount = frameCount + 1
    local currentTime = tick()
    
    if currentTime - lastTime >= 1 then -- Atualiza a cada segundo
        fps = frameCount
        frameCount = 0
        lastTime = currentTime
        
        -- Cor baseada no FPS
        local fpsColor = Color3.fromRGB(255, 0, 0) -- Vermelho (ruim)
        if fps >= 30 then
            fpsColor = Color3.fromRGB(255, 255, 0) -- Amarelo (médio)
        end
        if fps >= 60 then
            fpsColor = Color3.fromRGB(0, 255, 0) -- Verde (bom)
        end
        
        fpsLabel.Text = "FPS: " .. fps
        fpsLabel.TextColor3 = fpsColor
    end
end

-- Loop principal de atualização
RunService.Heartbeat:Connect(function()
    updateFPS()
    updatePing()
end)

print("Script Ping/FPS Monitor carregado com sucesso!")
print("- Clique e arraste para mover pela tela")
print("- Interface compacta e otimizada")
