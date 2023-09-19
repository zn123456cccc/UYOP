local OrionLib = loadstring(game:HttpGet(('https://raw.githubusercontent.com/shlexware/Orion/main/source')))()
local Window = OrionLib:MakeWindow({Name = "blade ball", HidePremium = false, lntroText = "Goldenhub", SaveConfig = true, ConfigFolder = "OrionTest"})

local Tab = Window:MakeTab({
	Name = "OP script",
	Icon = "rbxassetid://4483345998",
	PremiumOnly = false
})


OrionLib:MakeNotification({
	Name = "",
	Content = "ขอให้สนุก🥰",
	Image = "rbxassetid://4483345998",
	Time = 5
})


Tab:AddButton({
	Name = "auto block5/10",
	Callback = function()
      		local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local replicatedStorage = game:GetService("ReplicatedStorage")
local runService = game:GetService("RunService")
local parryButtonPress = replicatedStorage.Remotes.ParryButtonPress
local ballsFolder = workspace:WaitForChild("Balls")
 
print("Script successfully ran.")
 
local function onCharacterAdded(newCharacter)
    character = newCharacter
end
 
player.CharacterAdded:Connect(onCharacterAdded)
 
local focusedBall = nil  
 
local function chooseNewFocusedBall()
    local balls = ballsFolder:GetChildren()
    focusedBall = nil
    for _, ball in ipairs(balls) do
        if ball:GetAttribute("realBall") == true then
            focusedBall = ball
            break
        end
    end
end
 
chooseNewFocusedBall()
 
local function timeUntilImpact(ballVelocity, distanceToPlayer, playerVelocity)
    local directionToPlayer = (character.HumanoidRootPart.Position - focusedBall.Position).Unit
    local velocityTowardsPlayer = ballVelocity:Dot(directionToPlayer) - playerVelocity:Dot(directionToPlayer)
 
    if velocityTowardsPlayer <= 0 then
        return math.huge
    end
 
    local distanceToBeCovered = distanceToPlayer - 40
    return distanceToBeCovered / velocityTowardsPlayer
end
 
local BASE_THRESHOLD = 0.15
local VELOCITY_SCALING_FACTOR = 0.002
 
local function getDynamicThreshold(ballVelocityMagnitude)
    local adjustedThreshold = BASE_THRESHOLD - (ballVelocityMagnitude * VELOCITY_SCALING_FACTOR)
    return math.max(0.12, adjustedThreshold)
end
 
local function checkBallDistance()
    if not character:FindFirstChild("Highlight") then return end
    local charPos = character.PrimaryPart.Position
    local charVel = character.PrimaryPart.Velocity
 
    if focusedBall and not focusedBall.Parent then
        chooseNewFocusedBall()
    end
 
    if not focusedBall then return end
 
    local ball = focusedBall
    local distanceToPlayer = (ball.Position - charPos).Magnitude
 
    if distanceToPlayer < 10 then
        parryButtonPress:Fire()
        return
    end
 
    local timeToImpact = timeUntilImpact(ball.Velocity, distanceToPlayer, charVel)
    local dynamicThreshold = getDynamicThreshold(ball.Velocity.Magnitude)
 
    if timeToImpact < dynamicThreshold then
        parryButtonPress:Fire()
    end
end
 
 
 
runService.Heartbeat:Connect(function()
    checkBallDistance()
end)

  	end    
})

Tab:AddButton({
	Name = "ออโต้บล็อก8/10(ครบ8ตาขึ้นไปออกเกมเข้าใหม่)",
	Callback = function()
       	loadstring(game:HttpGet("https://raw.githubusercontent.com/Hosvile/Refinement/main/MC%3ABlade%20Ball%20Parry",true))()
  	end    
})

Tab:AddButton({
	Name = "ออโต้บล็อควงกลมสีแดง,6.5/10",
	Callback = function()
      		loadstring(game:HttpGet("https://raw.githubusercontent.com/1f0yt/community/main/Circle"))()
  	end    
})
Tab:AddButton({
	Name = "ออโต้บล็อคลื่นๆแต่กาก",
	Callback = function()
      		local Debug = false -- Set this to true if you want my debug output.
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Players = game:GetService("Players")
 
local Player = Players.LocalPlayer or Players.PlayerAdded:Wait()
local Remotes = ReplicatedStorage:WaitForChild("Remotes", 9e9) -- A second argument in waitforchild what could it mean?
local Balls = workspace:WaitForChild("Balls", 9e9)
 
-- Functions
 
local function print(...) -- Debug print.
	if Debug then
		warn(...)
	end
end
 
local function VerifyBall(Ball) -- Returns nil if the ball isn't a valid projectile; true if it's the right ball.
	if typeof(Ball) == "Instance" and Ball:IsA("BasePart") and Ball:IsDescendantOf(Balls) and Ball:GetAttribute("realBall") == true then
		return true
	end
end
 
local function IsTarget() -- Returns true if we are the current target.
	return (Player.Character and Player.Character:FindFirstChild("Highlight"))
end
 
local function Parry() -- Parries.
	Remotes:WaitForChild("ParryButtonPress"):Fire()
end
 
-- The actual code
 
Balls.ChildAdded:Connect(function(Ball)
	if not VerifyBall(Ball) then
		return
	end
 
	print(`Ball Spawned: {Ball}`)
 
	local OldPosition = Ball.Position
	local OldTick = tick()
 
	Ball:GetPropertyChangedSignal("Position"):Connect(function()
		if IsTarget() then -- No need to do the math if we're not being attacked.
			local Distance = (Ball.Position - workspace.CurrentCamera.Focus.Position).Magnitude 
			local Velocity = (OldPosition - Ball.Position).Magnitude -- Fix for .Velocity not working. Yes I got the lowest possible grade in accuplacer math.
 
			print(`Distance: {Distance}\nVelocity: {Velocity}\nTime: {Distance / Velocity}`)
 
			if (Distance / Velocity) <= 10 then -- Sorry for the magic number. This just works. No, you don't get a slider for this because it's 2am.
				Parry()
			end
		end
 
		if (tick() - OldTick >= 1/60) then -- Don't want it to update too quickly because my velocity implementation is aids. Yes, I tried Ball.Velocity. No, it didn't work.
			OldTick = tick()
			OldPosition = Ball.Position
		end
	end)
end)
  	end    
})

Tab:AddButton({
	Name = "อันนี้สำหรับคนเครื่องแรง(ออโต้บล็อค3/10)",
	Callback = function()
      		getgenv().ScriptConfig = {
    -- Distance in stud before the automatic triggers,
    -- you may change even after you have ran the script if you desire!
    -- Just be sure to REMOVE the loadstring, or else you will face some... issues
	DistanceBeforeParry = 46,
}
loadstring(game:HttpGet("https://raw.githubusercontent.com/Sussy-Tech/Scripts/main/BladeBall.lua"))()
  	end    
})


Tab:AddButton({
	Name = "วาปไปตี10/10ไงก็ชนะ",
	Callback = function()
      		getgenv().god = true
while getgenv().god and task.wait() do
    for _,ball in next, workspace.Balls:GetChildren() do
        if ball then
            if game:GetService("Players").LocalPlayer.Character and game:GetService("Players").LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
                game:GetService("Players").LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(game:GetService("Players").LocalPlayer.Character.HumanoidRootPart.Position, ball.Position)
                if game:GetService("Players").LocalPlayer.Character:FindFirstChild("Highlight") then
                    game:GetService("Players").LocalPlayer.Character.HumanoidRootPart.CFrame = ball.CFrame * CFrame.new(0, 0, (ball.Velocity).Magnitude * -0.5)
                    game:GetService("ReplicatedStorage").Remotes.ParryButtonPress:Fire()
                end
            end
        end
    end
end
  	end    
})

local Tab = Window:MakeTab({
	Name = "ฟังชั่นพิเศษ",
	Icon = "rbxassetid://4483345998",
	PremiumOnly = false
})



Tab:AddButton({
	Name = "เขียนชื่อตนและมันจะเข้าไปสิง",
	Callback = function()
      		loadstring(game:HttpGet("https://pastebin.com/raw/saMtiek2",true))()
  	end    
})


Tab:AddButton({
	Name = "บิน",
	Callback = function()
      		loadstring("\108\111\97\100\115\116\114\105\110\103\40\103\97\109\101\58\72\116\116\112\71\101\116\40\40\39\104\116\116\112\115\58\47\47\103\105\115\116\46\103\105\116\104\117\98\117\115\101\114\99\111\110\116\101\110\116\46\99\111\109\47\109\101\111\122\111\110\101\89\84\47\98\102\48\51\55\100\102\102\57\102\48\97\55\48\48\49\55\51\48\52\100\100\100\54\55\102\100\99\100\51\55\48\47\114\97\119\47\101\49\52\101\55\52\102\52\50\53\98\48\54\48\100\102\53\50\51\51\52\51\99\102\51\48\98\55\56\55\48\55\52\101\98\51\99\53\100\50\47\97\114\99\101\117\115\37\50\53\50\48\120\37\50\53\50\48\102\108\121\37\50\53\50\48\50\37\50\53\50\48\111\98\102\108\117\99\97\116\111\114\39\41\44\116\114\117\101\41\41\40\41\10\10")()
  	end    
})
OrionLib:Init() 
