-- @NoobZinx - GERADOR DE KEYS (COMPLETO)
local P = game:GetService("Players")
local LP = P.LocalPlayer
local HttpService = game:GetService("HttpService")

if not _G.KeysDatabase then
    _G.KeysDatabase = {}
end

-- ═══════════════════════════════════════════════════════
-- 🔐 SISTEMA DE GERAÇÃO DE KEYS OFFLINE
-- ═══════════════════════════════════════════════════════
local CHAVE_SECRETA = "NoobZinxSecretKey2024"

-- Função para codificar/decodificar usando XOR
local function codificarXOR(texto, chave)
    local resultado = {}
    for i = 1, #texto do
        local byte = string.byte(texto, i)
        local chaveByte = string.byte(chave, ((i - 1) % #chave) + 1)
        table.insert(resultado, string.char(bit32.bxor(byte, chaveByte)))
    end
    return table.concat(resultado)
end

-- Função para codificar em Base64
local function codificarBase64(dados)
    local b = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/'
    return ((dados:gsub('.', function(x) 
        local r,b='',x:byte()
        for i=8,1,-1 do r=r..(b%2^i-b%2^(i-1)>0 and '1' or '0') end
        return r;
    end)..'0000'):gsub('%d%d%d?%d?%d?%d?%d?', function(x)
        if (#x < 6) then return '' end
        local c=0
        for i=1,6 do c=c+(x:sub(i,i)=='1' and 2^(6-i) or 0) end
        return b:sub(c+1,c+1)
    end)..({ '', '==', '=' })[#dados%3+1])
end

-- Função para gerar Key válida entre dispositivos
local function gerarKeyValida(tipo, duracao)
    local dados = {
        tipo = tipo,
        duracao = duracao, -- em segundos (-1 para VIP permanente)
        dataCriacao = os.time(),
        dataExpiracao = nil
    }
    
    if tipo ~= "VIP Permanente" then
        dados.dataExpiracao = os.time() + duracao
    end
    
    local dadosJSON = HttpService:JSONEncode(dados)
    local dadosXOR = codificarXOR(dadosJSON, CHAVE_SECRETA)
    local keyFinal = codificarBase64(dadosXOR)
    
    return keyFinal
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
    local duracao = 0
    
    if tipo == "VIP Permanente" then
        duracao = -1
    else
        local dias = tonumber(string.match(tipo, "(%d+)")) or 1
        duracao = dias * 86400 -- 86400 segundos = 1 dia
    end
    
    local key = gerarKeyValida(tipo, duracao)
    keyAtual = key
    
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
