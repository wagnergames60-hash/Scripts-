# Scripts--- [[ SUPREME ESP ONLY OUTLINE & TRACERS ]]
local player = game.Players.LocalPlayer
local coreGui = game:GetService("CoreGui")
local runService = game:GetService("RunService")

-- Configurações
local ESP_COLOR = Color3.fromRGB(0, 255, 0) -- Verde
local LINE_WIDTH = 2 -- Grossura da linha

local function createESP(targetPlayer)
    if targetPlayer == player then return end

    local function setupCharacter(character)
        if not character then return end
        
        -- 1. CONTORNO (Highlight) - Só o contorno e visível através de paredes
        local highlight = character:FindFirstChild("SupremeESP") or Instance.new("Highlight")
        highlight.Name = "SupremeESP"
        highlight.Parent = character
        highlight.FillTransparency = 1 -- [MUDANÇA] Torna o corpo invisível (apenas contorno)
        highlight.OutlineTransparency = 0 -- Contorno visível
        highlight.OutlineColor = ESP_COLOR
        highlight.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop -- [MUDANÇA] Ver através de paredes

        -- 2. LINHAS (Tracers) usando Beams para garantir que apareçam
        local root = character:WaitForChild("HumanoidRootPart", 5)
        if not root then return end

        local myRoot = player.Character and player.Character:FindFirstChild("HumanoidRootPart")
        
        -- Criamos dois anexos para a linha se prender
        local att0 = Instance.new("Attachment", root) -- No inimigo
        local att1 = Instance.new("Attachment") -- Em você (será movido no loop)
        att1.Parent = workspace.Terrain -- Fica em um lugar neutro

        local beam = Instance.new("Beam", root)
        beam.Name = "SupremeTracer"
        beam.Color = ColorSequence.new(ESP_COLOR)
        beam.Width0 = LINE_WIDTH / 10
        beam.Width1 = LINE_WIDTH / 10
        beam.FaceCamera = true
        beam.Attachment0 = att0
        beam.Attachment1 = att1
        beam.Enabled = true

        local connection
        connection = runService.RenderStepped:Connect(function()
            if not character.Parent or not targetPlayer.Parent then
                beam:Destroy()
                att0:Destroy()
                att1:Destroy()
                connection:Disconnect()
                return
            end

            if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
                -- Atualiza a ponta da linha para a sua barriga
                att1.Position = player.Character.HumanoidRootPart.Position
                beam.Enabled = true
            else
                beam.Enabled = false
            end
        end)
    end

    targetPlayer.CharacterAdded:Connect(setupCharacter)
    if targetPlayer.Character then setupCharacter(targetPlayer.Character) end
end

-- Inicializar
for _, p in pairs(game.Players:GetPlayers()) do createESP(p) end
game.Players.PlayerAdded:Connect(createESP)

print("ESP Outline e Beams Ativados!")
