-- @NoobZinx - GERADOR DE KEYS (COMPLETO)
local P = game:GetService("Players")
local LP = P.LocalPlayer

if not _G.KeysDatabase then
    _G.KeysDatabase = {}
end

-- Função para calcular hash (mesma do painel)
local function calcularHash(key)
    local hash = 0
    for i = 1, #key do
        hash = (hash * 31 + string.byte(key, i)) % 1000000007
    end
    return hash
end

-- Função para gerar código aleatório válido
local function gerarCodigoValido(tipo)
    local caracteres = "ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789"
    local codigo = ""
    local tentativas = 0
    
    repeat
        codigo = ""
        for i = 1, 8 do
            local idx = math.random(1, #caracteres)
            codigo = codigo .. string.sub(caracteres, idx, idx)
        end
        
        local keyCompleta = "NOOBZINX-" .. tipo .. "-" .. codigo
        local hash = calcularHash(keyCompleta)
        local digitoVerificador = hash % 10
        
        if tipo == "VIP" then
            -- VIP precisa ter dígito verificador par
            if digitoVerificador % 2 == 0 then
                break
            end
        else
            -- Temporária precisa ter dígito verificador ímpar
            if digitoVerificador % 2 ~= 0 then
                break
            end
        end
        
        tentativas = tentativas + 1
    until tentativas > 100
    
    return codigo
end

-- Função para gerar Key única
local function gerarKeyUnica(tipo)
    local sufixo = ""
    
    if tipo == "VIP Permanente" then
        sufixo = "VIP"
    else
        local dias = string.match(tipo, "(%d+)")
        if dias then
            sufixo = dias .. "D"
        else
            sufixo = "1D"
        end
    end
    
    local key = ""
    local tentativas = 0
    repeat
        local codigo = gerarCodigoValido(sufixo)
        key = "NOOBZINX-" .. sufixo .. "-" .. codigo
        tentativas = tentativas + 1
        if tentativas > 100 then
            break
        end
    until _G.KeysDatabase[key] == nil
    
    return key
end

local SG = Instance.new("ScreenGui")
SG.Parent = LP.PlayerGui
SG.ResetOnSpawn = false
SG.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

local BotaoAbrir = Instance.new("TextButton")
BotaoAbrir.Parent = SG
BotaoAbrir.BackgroundColor3 = Color3.fromRGB(0, 100, 255)
BotaoAbrir.Size = UDim2.new(0, 60, 0, 60)
BotaoAbrir.Position = UDim2.new(0, 10, 0, 70)
BotaoAbrir.Text = "🔑"
BotaoAbrir.TextColor3 = Color3.fromRGB(255,255,255)
BotaoAbrir.TextScaled = true
BotaoAbrir.Font = Enum.Font.GothamBold
BotaoAbrir.Visible = false
Instance.new("UICorner",BotaoAbrir).CornerRadius = UDim.new(0, 12)
BotaoAbrir.ZIndex = 10

local MF = Instance.new("Frame")
MF.Parent = SG
MF.BackgroundColor3 = Color3.fromRGB(10,10,10)
MF.Size = UDim2.new(0, 350, 0, 380)
MF.Position = UDim2.new(0.5, -175, 0.5, -190)
MF.Active = true
MF.Draggable = true
MF.BorderSizePixel = 2
MF.BorderColor3 = Color3.fromRGB(0, 150, 255)
Instance.new("UICorner",MF).CornerRadius = UDim.new(0, 12)
MF.ZIndex = 5
MF.ClipsDescendants = false

local TitleBar = Instance.new("Frame")
TitleBar.Parent = MF
TitleBar.BackgroundColor3 = Color3.fromRGB(0, 120, 255)
TitleBar.Size = UDim2.new(1, 0, 0, 36)
TitleBar.BorderSizePixel = 0
Instance.new("UICorner",TitleBar).CornerRadius = UDim.new(0, 12)
TitleBar.ZIndex = 6

local Title = Instance.new("TextLabel")
Title.Parent = TitleBar
Title.BackgroundTransparency = 1
Title.Size = UDim2.new(0.7, 0, 1, 0)
Title.Position = UDim2.new(0.05, 0, 0, 0)
Title.Text = "🔑 GERADOR DE KEYS"
Title.TextColor3 = Color3.fromRGB(255,255,255)
Title.TextScaled = true
Title.Font = Enum.Font.GothamBold
Title.ZIndex = 6

local MinBtn = Instance.new("TextButton")
MinBtn.Parent = TitleBar
MinBtn.BackgroundColor3 = Color3.fromRGB(200, 150, 0)
MinBtn.Size = UDim2.new(0, 30, 1, 0)
MinBtn.Position = UDim2.new(1, -65, 0, 0)
MinBtn.Text = "─"
MinBtn.TextColor3 = Color3.fromRGB(255,255,255)
MinBtn.TextScaled = true
MinBtn.Font = Enum.Font.GothamBold
MinBtn.BorderSizePixel = 0
Instance.new("UICorner",MinBtn).CornerRadius = UDim.new(0, 8)
MinBtn.ZIndex = 6

local CloseBtn = Instance.new("TextButton")
CloseBtn.Parent = TitleBar
CloseBtn.BackgroundColor3 = Color3.fromRGB(200, 0, 0)
CloseBtn.Size = UDim2.new(0, 30, 1, 0)
CloseBtn.Position = UDim2.new(1, -32, 0, 0)
CloseBtn.Text = "✕"
CloseBtn.TextColor3 = Color3.fromRGB(255,255,255)
CloseBtn.TextScaled = true
CloseBtn.Font = Enum.Font.GothamBold
CloseBtn.BorderSizePixel = 0
Instance.new("UICorner",CloseBtn).CornerRadius = UDim.new(0, 8)
CloseBtn.ZIndex = 6

local Conteudo = Instance.new("Frame")
Conteudo.Parent = MF
Conteudo.BackgroundTransparency = 1
Conteudo.Size = UDim2.new(1, 0, 1, -36)
Conteudo.Position = UDim2.new(0, 0, 0, 36)
Conteudo.ZIndex = 5
Conteudo.ClipsDescendants = false

local tiposKeys = {"VIP Permanente", "1 Dia (24h)", "2 Dias", "3 Dias", "4 Dias", "5 Dias", "6 Dias", "7 Dias"}
local tipoSelecionado = "VIP Permanente"

local lblTipo = Instance.new("TextLabel")
lblTipo.Parent = Conteudo
lblTipo.BackgroundTransparency = 1
lblTipo.Size = UDim2.new(0.8, 0, 0, 25)
lblTipo.Position = UDim2.new(0.1, 0, 0, 15)
lblTipo.Text = "TIPO DE KEY"
lblTipo.TextColor3 = Color3.fromRGB(0, 200, 255)
lblTipo.TextScaled = true
lblTipo.Font = Enum.Font.GothamBold
lblTipo.ZIndex = 5

local DropdownBtn = Instance.new("TextButton")
DropdownBtn.Parent = Conteudo
DropdownBtn.BackgroundColor3 = Color3.fromRGB(30,30,30)
DropdownBtn.Size = UDim2.new(0.7, 0, 0, 32)
DropdownBtn.Position = UDim2.new(0.15, 0, 0, 45)
DropdownBtn.Text = "VIP Permanente"
DropdownBtn.TextColor3 = Color3.fromRGB(255,255,255)
DropdownBtn.TextScaled = true
DropdownBtn.Font = Enum.Font.Gotham
Instance.new("UICorner",DropdownBtn).CornerRadius = UDim.new(0, 6)
DropdownBtn.ZIndex = 7

local DropdownList = Instance.new("Frame")
DropdownList.Parent = Conteudo
DropdownList.BackgroundColor3 = Color3.fromRGB(25,25,25)
DropdownList.Size = UDim2.new(0.7, 0, 0, 0)
DropdownList.Position = UDim2.new(0.15, 0, 0, 79)
DropdownList.Visible = false
DropdownList.ClipsDescendants = true
Instance.new("UICorner",DropdownList).CornerRadius = UDim.new(0, 6)
DropdownList.ZIndex = 10

for i, opt in ipairs(tiposKeys) do
    local optBtn = Instance.new("TextButton")
    optBtn.Parent = DropdownList
    optBtn.BackgroundColor3 = i % 2 == 0 and Color3.fromRGB(35,35,35) or Color3.fromRGB(40,40,40)
    optBtn.Size = UDim2.new(1, 0, 0, 28)
    optBtn.Position = UDim2.new(0, 0, 0, (i-1)*28)
    optBtn.Text = opt
    optBtn.TextColor3 = Color3.fromRGB(255,255,255)
    optBtn.TextScaled = true
    optBtn.Font = Enum.Font.Gotham
    optBtn.BorderSizePixel = 0
    optBtn.ZIndex = 10
    optBtn.MouseButton1Click:Connect(function()
        tipoSelecionado = opt
        DropdownBtn.Text = opt
        DropdownList.Visible = false
        DropdownList.Size = UDim2.new(0.7, 0, 0, 0)
    end)
end

DropdownBtn.MouseButton1Click:Connect(function()
    if DropdownList.Visible then
        DropdownList.Visible = false
        DropdownList.Size = UDim2.new(0.7, 0, 0, 0)
    else
        DropdownList.Visible = true
        DropdownList.Size = UDim2.new(0.7, 0, 0, #tiposKeys * 28)
    end
end)

local KeyGerada = Instance.new("TextLabel")
KeyGerada.Parent = Conteudo
KeyGerada.BackgroundColor3 = Color3.fromRGB(20,20,20)
KeyGerada.Size = UDim2.new(0.8, 0, 0, 35)
KeyGerada.Position = UDim2.new(0.1, 0, 0, 130)
KeyGerada.Text = "🔑 Clique em GERAR"
KeyGerada.TextColor3 = Color3.fromRGB(200,200,200)
KeyGerada.TextScaled = true
KeyGerada.Font = Enum.Font.Gotham
Instance.new("UICorner",KeyGerada).CornerRadius = UDim.new(0, 6)
KeyGerada.ZIndex = 5

local BtnGerar = Instance.new("TextButton")
BtnGerar.Parent = Conteudo
BtnGerar.BackgroundColor3 = Color3.fromRGB(0, 200, 0)
BtnGerar.Size = UDim2.new(0.6, 0, 0, 40)
BtnGerar.Position = UDim2.new(0.2, 0, 0, 180)
BtnGerar.Text = "GERAR KEY"
BtnGerar.TextColor3 = Color3.fromRGB(255,255,255)
BtnGerar.TextScaled = true
BtnGerar.Font = Enum.Font.GothamBold
Instance.new("UICorner",BtnGerar).CornerRadius = UDim.new(0, 8)
BtnGerar.ZIndex = 5

local BtnCopiar = Instance.new("TextButton")
BtnCopiar.Parent = Conteudo
BtnCopiar.BackgroundColor3 = Color3.fromRGB(0, 100, 255)
BtnCopiar.Size = UDim2.new(0.6, 0, 0, 35)
BtnCopiar.Position = UDim2.new(0.2, 0, 0, 230)
BtnCopiar.Text = "COPIAR KEY"
BtnCopiar.TextColor3 = Color3.fromRGB(255,255,255)
BtnCopiar.TextScaled = true
BtnCopiar.Font = Enum.Font.GothamBold
Instance.new("UICorner",BtnCopiar).CornerRadius = UDim.new(0, 8)
BtnCopiar.ZIndex = 5

local keyAtual = ""

BtnGerar.MouseButton1Click:Connect(function()
    local tipo = tipoSelecionado
    local key = gerarKeyUnica(tipo)
    keyAtual = key
    
    local tempo = 0
    if tipo == "VIP Permanente" then
        tempo = -1
    else
        local dias = tonumber(string.match(tipo, "(%d+)")) or 1
        tempo = dias * 86400
    end
    
    _G.KeysDatabase[key] = {
        tipo = tipo,
        tempoRestante = tempo,
        usada = false,
        valida = true,
        dataCriacao = os.time()
    }
    
    KeyGerada.Text = "🔑 " .. key
    KeyGerada.TextColor3 = Color3.fromRGB(0, 255, 100)
end)

BtnCopiar.MouseButton1Click:Connect(function()
    if keyAtual ~= "" then
        setclipboard(keyAtual)
        KeyGerada.Text = "📋 " .. keyAtual .. " (COPIADO!)"
        KeyGerada.TextColor3 = Color3.fromRGB(0, 200, 255)
        task.wait(2)
        KeyGerada.Text = "🔑 " .. keyAtual
        KeyGerada.TextColor3 = Color3.fromRGB(0, 255, 100)
    else
        KeyGerada.Text = "❌ Nenhuma Key para copiar"
        KeyGerada.TextColor3 = Color3.fromRGB(255, 0, 0)
        task.wait(1)
        KeyGerada.Text = "🔑 Clique em GERAR"
        KeyGerada.TextColor3 = Color3.fromRGB(200,200,200)
    end
end)

local minimizado = false

MinBtn.MouseButton1Click:Connect(function()
    minimizado = not minimizado
    if minimizado then
        Conteudo.Visible = false
        MF.Size = UDim2.new(0, 350, 0, 36)
        MF.Position = UDim2.new(0.5, -175, 0.5, -18)
        MinBtn.Text = "□"
    else
        Conteudo.Visible = true
        MF.Size = UDim2.new(0, 350, 0, 380)
        MF.Position = UDim2.new(0.5, -175, 0.5, -190)
        MinBtn.Text = "─"
    end
end)

CloseBtn.MouseButton1Click:Connect(function()
    MF.Visible = false
    BotaoAbrir.Visible = true
end)

BotaoAbrir.MouseButton1Click:Connect(function()
    MF.Visible = true
    BotaoAbrir.Visible = false
end)

print("✅ GERADOR DE KEYS CARREGADO!")
