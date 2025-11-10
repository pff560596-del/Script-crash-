# Script-crash--- AdminActions (ServerScriptService)
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Players = game:GetService("Players")

local event = ReplicatedStorage:WaitForChild("AdminActionEvent")

-- Lista de admins (pode usar UserId em vez de nomes)
local AdminUserIds = {
    [12345678] = true, -- substitua pelo seu UserId
    -- [87654321] = true,
}

-- Função segura para "simular crash" -> aqui: kick com mensagem
local function performSafeAction(requestingPlayer, targetName)
    -- Verifica se quem solicitou é admin
    if not AdminUserIds[requestingPlayer.UserId] then
        warn(("Player %s tentou usar AdminAction sem permissão"):format(requestingPlayer.Name))
        return
    end

    -- Busca jogador alvo pelo nome (case-insensitive)
    local targetPlayer = nil
    for _, p in pairs(Players:GetPlayers()) do
        if string.lower(p.Name) == string.lower(targetName) then
            targetPlayer = p
            break
        end
    end

    if not targetPlayer then
        -- opcional: enviar feedback ao solicitante via RemoteEvent (não implementado aqui)
        warn("Alvo não encontrado: " .. tostring(targetName))
        return
    end

    -- AÇÃO SEGURA: kick com mensagem (USAR COM RESPONSABILIDADE)
    local reason = "Ação administrativa: simulação de crash (seguro)."
    targetPlayer:Kick(reason)
    -- Alternativas seguras: targetPlayer:LoadCharacter() (respawn), targetPlayer.Character.Humanoid.Health = 0 (matar)
end

-- Conectar evento remoto
event.OnServerEvent:Connect(function(player, action, payload)
    -- action: string ("simulate_crash", etc)
    if action == "simulate_crash" and type(payload) == "string" then
        performSafeAction(player, payload)
    else
        warn(("Requisição inválida de %s"):format(player.Name))
    end
end)
-- AdminActions (ServerScriptService)
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Players = game:GetService("Players")

local event = ReplicatedStorage:WaitForChild("AdminActionEvent")

-- Lista de admins (pode usar UserId em vez de nomes)
local AdminUserIds = {
    [12345678] = true, -- substitua pelo seu UserId
    -- [87654321] = true,
}

-- Função segura para "simular crash" -> aqui: kick com mensagem
local function performSafeAction(requestingPlayer, targetName)
    -- Verifica se quem solicitou é admin
    if not AdminUserIds[requestingPlayer.UserId] then
        warn(("Player %s tentou usar AdminAction sem permissão"):format(requestingPlayer.Name))
        return
    end

    -- Busca jogador alvo pelo nome (case-insensitive)
    local targetPlayer = nil
    for _, p in pairs(Players:GetPlayers()) do
        if string.lower(p.Name) == string.lower(targetName) then
            targetPlayer = p
            break
        end
    end

    if not targetPlayer then
        -- opcional: enviar feedback ao solicitante via RemoteEvent (não implementado aqui)
        warn("Alvo não encontrado: " .. tostring(targetName))
        return
    end

    -- AÇÃO SEGURA: kick com mensagem (USAR COM RESPONSABILIDADE)
    local reason = "Ação administrativa: simulação de crash (seguro)."
    targetPlayer:Kick(reason)
    -- Alternativas seguras: targetPlayer:LoadCharacter() (respawn), targetPlayer.Character.Humanoid.Health = 0 (matar)
end

-- Conectar evento remoto
event.OnServerEvent:Connect(function(player, action, payload)
    -- action: string ("simulate_crash", etc)
    if action == "simulate_crash" and type(payload) == "string" then
        performSafeAction(player, payload)
    else
        warn(("Requisição inválida de %s"):format(player.Name))
    end
end)
-- AdminPanelClient (LocalScript dentro do ScreenGui)
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Players = game:GetService("Players")

local event = ReplicatedStorage:WaitForChild("AdminActionEvent")

local player = Players.LocalPlayer
local screenGui = script.Parent
-- Ajuste os caminhos abaixo de acordo com sua hierarquia de GUI:
local frame = screenGui:WaitForChild("Frame")
local targetBox = frame:WaitForChild("TargetTextBox") -- TextBox para digitar nome do jogador
local button = frame:WaitForChild("CrashButton") -- TextButton com texto "Crash Player"

-- Ao clicar no botão, envia ao servidor a ação "simulate_crash" com o nome alvo
button.MouseButton1Click:Connect(function()
    local targetName = targetBox.Text or ""
    if targetName == "" then
        -- opcional: mostrar aviso na GUI
        print("Digite o nome do jogador alvo.")
        return
    end

    -- Envia pedido ao servidor (servidor decide se procede)
    event:FireServer("simulate_crash", targetName)
end)
