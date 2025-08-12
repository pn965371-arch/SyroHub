
--[[
SYRO HUB - Vox Seas Multi-Funções
Base completa, funções organizadas por abas e botões/toggles/menus/sliders
Comente e ajuste as funções conforme descobrir nomes/estruturas do jogo!
Feito para executores como Delta
]]

local CoreGui = game:GetService("CoreGui")
if CoreGui:FindFirstChild("SYRO_HUB") then CoreGui.SYRO_HUB:Destroy() end

local gui = Instance.new("ScreenGui", CoreGui)
gui.Name = "SYRO_HUB"
gui.ResetOnSpawn = false

-- Main frame
local mainFrame = Instance.new("Frame", gui)
mainFrame.Name = "mainFrame"
mainFrame.Size = UDim2.new(0, 670, 0, 540)
mainFrame.Position = UDim2.new(0.5, -335, 0.5, -270)
mainFrame.BackgroundColor3 = Color3.new(0, 0, 0)
mainFrame.BorderSizePixel = 0

local syroLabel = Instance.new("TextLabel", mainFrame)
syroLabel.Size = UDim2.new(1, 0, 0, 60)
syroLabel.Position = UDim2.new(0, 0, 0, 0)
syroLabel.BackgroundTransparency = 1
syroLabel.Text = "SYRO"
syroLabel.TextColor3 = Color3.fromRGB(220, 0, 0)
syroLabel.Font = Enum.Font.GothamBold
syroLabel.TextSize = 50

-- Função alternativa para table.find
local function tableFind(tbl, value)
    for i,v in ipairs(tbl) do
        if v == value then return i end
    end
    return nil
end

-- Abas e funções
local tabs = {
    ["Combate"] = {
        {"Auto Farm", "Farmar mobs básicos automaticamente", "toggle"},
        {"Auto Farm Boss", "Focar e atacar bosses automaticamente", "toggle"},
        {"Kill Aura", "Ataque automático em inimigos próximos", "toggle_slider"},
        {"Fast Click", "Clicar rápido para ataques", "toggle_slider"},
        {"Aimbot", "Mira automática em inimigos", "toggle"}
    },
    ["Eventos & Quests"] = {
        {"Auto Quest", "Aceitar e completar quests automaticamente", "toggle"},
        {"Auto Sea Beast", "Farmar e derrotar Sea Beasts automaticamente", "toggle"}
    },
    ["Frutas & Itens"] = {
        {"Fruit Spinner", "Girar roda de frutas automaticamente", "toggle"},
        {"Auto Status", "Distribuir pontos automaticamente", "toggle"},
        {"Auto Store Fruit", "Guardar frutas automaticamente", "toggle"},
        {"Open Fruit Dealer", "Abrir NPC vendedor de frutas", "button"}
    },
    ["Mobilidade"] = {
        {"Teleporte Ilha/NPC", "Teleporte para qualquer ilha ou NPC", "menu"},
        {"ServerHop", "Trocar de servidor automaticamente", "button"}
    },
    ["Personalização & Extras"] = {
        {"Fake Status", "Fingir status para outros jogadores", "toggle"},
        {"Auto Haki", "Usar Haki automaticamente", "toggle"},
        {"Unlock Weapons/Accessories", "Destravar armas e acessórios", "button"},
        {"Random Race", "Sortear raça do personagem", "button"}
    },
    ["Anti-Ban & Interface"] = {
        {"Anti-AFK", "Prevenir kick por inatividade", "toggle"},
        {"Safe Mode", "Modo seguro para evitar banimentos", "toggle"},
        {"GUI Limpa", "Reiniciar/Corrigir interface", "button"}
    }
}

local tabFrames = {}
local tabOrder = {}
for tab, _ in pairs(tabs) do table.insert(tabOrder, tab) end

for i, tab in ipairs(tabOrder) do
    local btn = Instance.new("TextButton", mainFrame)
    btn.Name = tab.."Btn"
    btn.Size = UDim2.new(0, 160, 0, 32)
    btn.Position = UDim2.new(0, (i-1)*165, 0, 70)
    btn.Text = tab
    btn.TextColor3 = Color3.fromRGB(220,0,0)
    btn.BackgroundColor3 = Color3.new(0.1,0.1,0.1)
    btn.Font = Enum.Font.GothamBold
    btn.TextSize = 16

    local tabFrame = Instance.new("Frame", mainFrame)
    tabFrame.Name = tab.."Frame"
    tabFrame.Size = UDim2.new(1, -20, 1, -120)
    tabFrame.Position = UDim2.new(0, 10, 0, 110)
    tabFrame.BackgroundTransparency = 1
    tabFrame.Visible = (i==1)
    tabFrames[tab] = tabFrame

    btn.MouseButton1Click:Connect(function()
        for _, fr in pairs(tabFrames) do fr.Visible = false end
        tabFrame.Visible = true
    end)
end

-------------------------
-- Adiciona botões/toggles/menus em cada aba
-------------------------
local toggles = {} -- Guarda estado dos toggles
local sliders = {} -- Guarda sliders/ranges

for tab, funcs in pairs(tabs) do
    local tabFrame = tabFrames[tab]
    for idx, func in ipairs(funcs) do
        local name, desc, type = unpack(func)
        local y = 10 + (idx-1)*50

        if type == "toggle" then
            -- Toggle simples ON/OFF
            local toggle = Instance.new("TextButton", tabFrame)
            toggle.Name = name.."Toggle"
            toggle.Size = UDim2.new(0,260,0,32)
            toggle.Position = UDim2.new(0, 10, 0, y)
            toggle.BackgroundColor3 = Color3.fromRGB(40,40,40)
            toggle.TextColor3 = Color3.fromRGB(220,0,0)
            toggle.Font = Enum.Font.GothamBold
            toggle.TextSize = 15
            toggle.Text = name.." [DESLIGADO]"
            toggles[name] = false

            toggle.MouseButton1Click:Connect(function()
                toggles[name] = not toggles[name]
                if toggles[name] then
                    toggle.Text = name.." [LIGADO]"
                    toggle.BackgroundColor3 = Color3.fromRGB(100,40,40)
                else
                    toggle.Text = name.." [DESLIGADO]"
                    toggle.BackgroundColor3 = Color3.fromRGB(40,40,40)
                end
            end)

            local descLabel = Instance.new("TextLabel", tabFrame)
            descLabel.Size = UDim2.new(0, 320, 0, 20)
            descLabel.Position = UDim2.new(0, 280, 0, y+6)
            descLabel.BackgroundTransparency = 1
            descLabel.Text = desc
            descLabel.TextColor3 = Color3.fromRGB(180,180,180)
            descLabel.Font = Enum.Font.Gotham
            descLabel.TextSize = 13
            descLabel.TextXAlignment = Enum.TextXAlignment.Left

        elseif type == "toggle_slider" then
            local toggle = Instance.new("TextButton", tabFrame)
            toggle.Name = name.."Toggle"
            toggle.Size = UDim2.new(0,180,0,32)
            toggle.Position = UDim2.new(0, 10, 0, y)
            toggle.BackgroundColor3 = Color3.fromRGB(40,40,40)
            toggle.TextColor3 = Color3.fromRGB(220,0,0)
            toggle.Font = Enum.Font.GothamBold
            toggle.TextSize = 15
            toggle.Text = name.." [DESLIGADO]"
            toggles[name] = false

            local sliderLabel = Instance.new("TextLabel", tabFrame)
            sliderLabel.Size = UDim2.new(0, 60, 0, 32)
            sliderLabel.Position = UDim2.new(0, 200, 0, y)
            sliderLabel.BackgroundTransparency = 1
            sliderLabel.Text = "Valor: 30"
            sliderLabel.TextColor3 = Color3.fromRGB(220,0,0)
            sliderLabel.Font = Enum.Font.GothamBold
            sliderLabel.TextSize = 13

            local sliderBtn = Instance.new("TextButton", tabFrame)
            sliderBtn.Size = UDim2.new(0, 60, 0, 32)
            sliderBtn.Position = UDim2.new(0, 270, 0, y)
            sliderBtn.BackgroundColor3 = Color3.fromRGB(40,40,40)
            sliderBtn.Text = "+/-"
            sliderBtn.TextColor3 = Color3.fromRGB(220,0,0)
            sliderBtn.Font = Enum.Font.GothamBold
            sliderBtn.TextSize = 13

            sliders[name] = 30

            sliderBtn.MouseButton1Click:Connect(function()
                if sliders[name] == 20 then sliders[name] = 30
                elseif sliders[name] == 30 then sliders[name] = 40
                elseif sliders[name] == 40 then sliders[name] = 50
                else sliders[name] = 20 end
                sliderLabel.Text = "Valor: "..sliders[name]
            end)

            toggle.MouseButton1Click:Connect(function()
                toggles[name] = not toggles[name]
                if toggles[name] then
                    toggle.Text = name.." [LIGADO]"
                    toggle.BackgroundColor3 = Color3.fromRGB(100,40,40)
                else
                    toggle.Text = name.." [DESLIGADO]"
                    toggle.BackgroundColor3 = Color3.fromRGB(40,40,40)
                end
            end)

            local descLabel = Instance.new("TextLabel", tabFrame)
            descLabel.Size = UDim2.new(0, 220, 0, 20)
            descLabel.Position = UDim2.new(0, 340, 0, y+6)
            descLabel.BackgroundTransparency = 1
            descLabel.Text = desc
            descLabel.TextColor3 = Color3.fromRGB(180,180,180)
            descLabel.Font = Enum.Font.Gotham
            descLabel.TextSize = 13
            descLabel.TextXAlignment = Enum.TextXAlignment.Left

        elseif type == "button" then
            local btn = Instance.new("TextButton", tabFrame)
            btn.Name = name.."Btn"
            btn.Size = UDim2.new(0,220,0,32)
            btn.Position = UDim2.new(0, 10, 0, y)
            btn.BackgroundColor3 = Color3.fromRGB(40,40,40)
            btn.TextColor3 = Color3.fromRGB(220,0,0)
            btn.Font = Enum.Font.GothamBold
            btn.TextSize = 15
            btn.Text = name

            local descLabel = Instance.new("TextLabel", tabFrame)
            descLabel.Size = UDim2.new(0, 320, 0, 20)
            descLabel.Position = UDim2.new(0, 240, 0, y+6)
            descLabel.BackgroundTransparency = 1
            descLabel.Text = desc
            descLabel.TextColor3 = Color3.fromRGB(180,180,180)
            descLabel.Font = Enum.Font.Gotham
            descLabel.TextSize = 13
            descLabel.TextXAlignment = Enum.TextXAlignment.Left

        elseif type == "menu" then
            local menuLabel = Instance.new("TextLabel", tabFrame)
            menuLabel.Size = UDim2.new(0, 200, 0, 32)
            menuLabel.Position = UDim2.new(0, 10, 0, y)
            menuLabel.BackgroundTransparency = 1
            menuLabel.Text = name.." - Selecione destino"
            menuLabel.TextColor3 = Color3.fromRGB(220,0,0)
            menuLabel.Font = Enum.Font.GothamBold
            menuLabel.TextSize = 13

            local dropdown = Instance.new("TextButton", tabFrame)
            dropdown.Name = name.."Dropdown"
            dropdown.Size = UDim2.new(0, 180, 0, 32)
            dropdown.Position = UDim2.new(0, 220, 0, y)
            dropdown.BackgroundColor3 = Color3.fromRGB(40,40,40)
            dropdown.TextColor3 = Color3.fromRGB(220,0,0)
            dropdown.Font = Enum.Font.GothamBold
            dropdown.TextSize = 15
            dropdown.Text = "Selecionar..."

            local destinos = {"Ilha Inicial", "Ilha dos Piratas", "Ilha dos Marines", "NPC Quest", "Boss"}
            local currentDestino = destinos[1]

            dropdown.MouseButton1Click:Connect(function()
                local idx = tableFind(destinos, currentDestino) or 1
                idx = idx + 1
                if idx > #destinos then idx = 1 end
                currentDestino = destinos[idx]
                dropdown.Text = currentDestino
                -- Aqui implementar lógica de teleport para o destino selecionado
            end)

            local descLabel = Instance.new("TextLabel", tabFrame)
            descLabel.Size = UDim2.new(0, 220, 0, 20)
            descLabel.Position = UDim2.new(0, 410, 0, y+6)
            descLabel.BackgroundTransparency = 1
            descLabel.Text = desc
            descLabel.TextColor3 = Color3.fromRGB(180,180,180)
            descLabel.Font = Enum.Font.Gotham
            descLabel.TextSize = 13
            descLabel.TextXAlignment = Enum.TextXAlignment.Left
        end
    end
end

-------------------------
-- Botão Fechar/Abrir e arrastar
-------------------------
local closeBtn = Instance.new("TextButton", mainFrame)
closeBtn.Size = UDim2.new(0, 32, 0, 32)
closeBtn.Position = UDim2.new(1, -42, 0, 8)
closeBtn.Text = "X"
closeBtn.TextColor3 = Color3.fromRGB(220,0,0)
closeBtn.BackgroundColor3 = Color3.new(0.2,0.2,0.2)
closeBtn.Font = Enum.Font.GothamBold
closeBtn.TextSize = 20
closeBtn.MouseButton1Click:Connect(function()
    mainFrame.Visible = false
    gui.OpenBtn.Visible = true
end)

local openBtn = Instance.new("TextButton", gui)
openBtn.Name = "OpenBtn"
openBtn.Size = UDim2.new(0, 60, 0, 60)
openBtn.Position = UDim2.new(0, 15, 0, 15)
openBtn.Text = "SYRO"
openBtn.TextColor3 = Color3.fromRGB(220,0,0)
openBtn.BackgroundColor3 = Color3.new(0,0,0)
openBtn.Font = Enum.Font.GothamBold
openBtn.TextSize = 32
openBtn.Visible = false
openBtn.MouseButton1Click:Connect(function()
    mainFrame.Visible = true
    openBtn.Visible = false
end)

local UIS = game:GetService("UserInputService")
local dragging, dragStart, startPos
mainFrame.Active = true
mainFrame.Selectable = true

mainFrame.InputBegan:Connect(function(input)
	if input.UserInputType == Enum.UserInputType.MouseButton1 then
		dragging = true
		dragStart = input.Position
		startPos = mainFrame.Position
		input.Changed:Connect(function()
			if input.UserInputState == Enum.UserInputState.End then
				dragging = false
			end
		end)
	end
end)

mainFrame.InputChanged:Connect(function(input)
	if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
		local delta = input.Position - dragStart
		mainFrame.Position = UDim2.new(
			startPos.X.Scale, startPos.X.Offset + delta.X,
			startPos.Y.Scale, startPos.Y.Offset + delta.Y
	 )
	end
end)

-------------------------
-- EXEMPLO DE LÓGICA DAS FUNÇÕES
spawn(function()
    while true do
        if toggles["Kill Aura"] then
            -- Aqui a lógica de detecção, range = sliders["Kill Aura"]
            -- local npcs = DetectarNPCsProximos(sliders["Kill Aura"])
            -- for _,npc in ipairs(npcs) do Atacar(npc) end
        end
        wait(0.15)
    end
end)
