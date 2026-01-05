local Close = Instance.new("TextButton")
local Open2 = Instance.new("ScreenGui")
local Open = Instance.new("TextButton")
local FightingGui = Instance.new("ScreenGui")
local main = Instance.new("Frame")
local Cre = Instance.new("TextLabel")
local HitBox = Instance.new("TextBox")

local UICorner = Instance.new("UICorner")
local UICorner_2 = Instance.new("UICorner")
local UICorner_3 = Instance.new("UICorner")
local UICorner_4 = Instance.new("UICorner")

-- GUI PRINCIPAL
FightingGui.Name = "FightingGui"
FightingGui.Parent = game.CoreGui
FightingGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
FightingGui.Enabled = false

-- BOTÃO ABRIR
Open2.Name = "Tools"
Open2.Parent = game.CoreGui

Open.Name = "abrir"
Open.Parent = Open2
Open.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
Open.Position = UDim2.new(0, 0, 0.45, 0)
Open.Size = UDim2.new(0, 50, 0, 50)
Open.Font = Enum.Font.FredokaOne
Open.Text = "Open"
Open.TextColor3 = Color3.fromRGB(255, 0, 0)
Open.TextScaled = true
Open.MouseButton1Down:Connect(function()
	FightingGui.Enabled = true
end)

-- FRAME
main.Parent = FightingGui
main.Active = true
main.BackgroundColor3 = Color3.fromRGB(42, 42, 42)
main.Position = UDim2.new(0.259, 0, 0.237, 0)
main.Size = UDim2.new(0, 127, 0, 111)
main.Draggable = true
UICorner.Parent = main

-- BOTÃO FECHAR
Close.Name = "Close"
Close.Parent = main
Close.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
Close.Position = UDim2.new(1, -30, 0, 0)
Close.Size = UDim2.new(0, 30, 0, 30)
Close.Font = Enum.Font.FredokaOne
Close.Text = "X"
Close.TextColor3 = Color3.fromRGB(0, 0, 0)
Close.TextScaled = true
Close.MouseButton1Down:Connect(function()
	FightingGui.Enabled = false
end)
UICorner_4.Parent = Close

-- TÍTULO
Cre.Name = "Cre"
Cre.Parent = main
Cre.BackgroundColor3 = Color3.fromRGB(34, 34, 34)
Cre.Size = UDim2.new(1, 0, 0, 16)
Cre.Font = Enum.Font.FredokaOne
Cre.Text = "Script made by WarriorRoberr"
Cre.TextScaled = true
Cre.TextColor3 = Color3.fromRGB(0, 0, 0)
UICorner_2.Parent = Cre

-- HITBOX INPUT
HitBox.Name = "HitBox"
HitBox.Parent = main
HitBox.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
HitBox.Position = UDim2.new(0.113, 0, 0.2, 0)
HitBox.Size = UDim2.new(0, 103, 0, 70)
HitBox.Font = Enum.Font.FredokaOne
HitBox.PlaceholderText = "Hitbox Size"
HitBox.Text = ""
HitBox.TextScaled = true
HitBox.TextColor3 = Color3.fromRGB(255, 255, 255)
UICorner_3.Parent = HitBox

-- SERVIÇOS
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer

-- RGB
local hue = 0

-- FUNÇÃO CRIAR BARRA DE VIDA
local function createHealthBar(hrp)
	if hrp:FindFirstChild("HealthBar") then return end

	local billboard = Instance.new("BillboardGui")
	billboard.Name = "HealthBar"
	billboard.Adornee = hrp
	billboard.Size = UDim2.new(0, 6, 0, 50)
	billboard.StudsOffset = Vector3.new(2.2, 0, 0)
	billboard.AlwaysOnTop = true
	billboard.Parent = hrp

	local bg = Instance.new("Frame", billboard)
	bg.Size = UDim2.new(1, 0, 1, 0)
	bg.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
	bg.BorderSizePixel = 0

	local fill = Instance.new("Frame", bg)
	fill.Name = "Fill"
	fill.Size = UDim2.new(1, 0, 1, 0)
	fill.BorderSizePixel = 0
end

-- ATUALIZAÇÃO
RunService.RenderStepped:Connect(function()
	local size = tonumber(HitBox.Text)
	if not size then return end

	hue += 0.01
	if hue > 1 then hue = 0 end

	-- RGB MAIS FORTE 🔥🔥
	local rgb = Color3.fromHSV(hue, 1, 0.9)

	for _, v in pairs(Players:GetPlayers()) do
		if v ~= LocalPlayer
		and v.Character
		and v.Character:FindFirstChild("HumanoidRootPart")
		and v.Character:FindFirstChild("Humanoid") then

			local hrp = v.Character.HumanoidRootPart
			local hum = v.Character.Humanoid

			-- HITBOX
			hrp.Size = Vector3.new(size, size, size)
			hrp.Transparency = 0.25
			hrp.Color = rgb
			hrp.Material = Enum.Material.Neon
			hrp.CanCollide = false

			-- BARRA DE VIDA
			createHealthBar(hrp)
			local bar = hrp.HealthBar.Frame.Fill
			local hp = math.clamp(hum.Health / hum.MaxHealth, 0, 1)
			bar.Size = UDim2.new(1, 0, hp, 0)

			if hp > 0.5 then
				bar.BackgroundColor3 = Color3.fromRGB(0, 255, 0)
			elseif hp > 0.2 then
				bar.BackgroundColor3 = Color3.fromRGB(255, 170, 0)
			else
				bar.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
			end
		end
	end
end)
