local ChangerFound = false
local ChangerRemote = nil
local ToppingFound = false
local ToppingRemote = nil
local FlattenFound = false
local FlattenRemote = nil

local s,f=pcall(function()
local Dough = game:GetService("Workspace").AllDough.Dough
for i,v in pairs(game:GetService("ReplicatedStorage").Communication.Events:GetChildren()) do
if not ChangerFound then
local SpecifiedAnchored = not Dough.Anchored
v:FireServer(Dough,"Anchored",SpecifiedAnchored)
wait(0.1)
if Dough.Anchored == SpecifiedAnchored then
   ChangerFound = true
   ChangerRemote = v
   print("CHANGER REMOTE FOUND")
end
end
end

for i,v in pairs(game:GetService("ReplicatedStorage").Communication.Events:GetChildren()) do
if not ToppingFound then
v:FireServer(Dough, "Cheese")
wait(0.1)
if Dough.SG.Frame:FindFirstChild("Cheese") then
   ToppingFound = true
   ToppingRemote = v
   print("TOPPING REMOTE FOUND")
end
end
end

for i,v in pairs(game:GetService("ReplicatedStorage").Communication.Events:GetChildren()) do
if not FlattenFound then
v:FireServer(Dough)
wait(0.1)
if not Dough:FindFirstChildOfClass("SpecialMesh") then
   FlattenFound = true
   FlattenRemote = v
   print("FLATTEN REMOTE FOUND")
end
end
end

function Change(Instance,Property,Value)
   ChangerRemote:FireServer(Instance,Property,Value)
end

function Topping(Instance,Topping)
   ToppingRemote:FireServer(Instance,Topping)
end

function Flatten(Instance)
   FlattenRemote:FireServer(Instance)
end

game:GetService("UserInputService").InputBegan:Connect(function(inputa,gp)
if gp then return end
if inputa.KeyCode == Enum.KeyCode.Q then
   Flatten(game.Players.LocalPlayer:GetMouse().Target)
end
if inputa.KeyCode == Enum.KeyCode.E then
   Topping(game.Players.LocalPlayer:GetMouse().Target,"Cheese")
   Topping(game.Players.LocalPlayer:GetMouse().Target,"TomatoSauce")
end
if inputa.KeyCode == Enum.KeyCode.R then
   Topping(game.Players.LocalPlayer:GetMouse().Target,"Pepperoni")
   Topping(game.Players.LocalPlayer:GetMouse().Target,"Cheese")
   Topping(game.Players.LocalPlayer:GetMouse().Target,"TomatoSauce")
end
if inputa.KeyCode == Enum.KeyCode.T then
   Topping(game.Players.LocalPlayer:GetMouse().Target,"Sausage")
   Topping(game.Players.LocalPlayer:GetMouse().Target,"Cheese")
   Topping(game.Players.LocalPlayer:GetMouse().Target,"TomatoSauce")
end
if inputa.KeyCode == Enum.KeyCode.Y then
   for i,v in pairs(game:GetService("Workspace").AllDough:GetChildren()) do
       if v:FindFirstChildOfClass("SpecialMesh") then
           Flatten(v)
           wait()
       end
   end
   for i,v in pairs(game:GetService("Workspace").AllDough:GetChildren()) do
           Topping(v,"TomatoSauce")
           wait()
   end
   for i,v in pairs(game:GetService("Workspace").AllDough:GetChildren()) do
           Topping(v,"Cheese")
           wait()
   end
   local OvenPositions = {
       CFrame.new(59.3907127, 8.87603855, 55),
       CFrame.new(59.3907127, 7.55361891, 60),
       CFrame.new(59.3907127, 7.55361891, 65),
       CFrame.new(59.3907127, 7.55361891, 50),
       CFrame.new(59.3907127, 1.8, 55),
       CFrame.new(59.3907127, 1.8, 60),
       CFrame.new(59.3907127, 1.8, 65),
       CFrame.new(59.3907127, 1.8, 50)
   }
   for i,v in pairs(game:GetService("Workspace").Ovens:GetChildren()) do
       if not v.IsOpen.Value then
           v.Door.ClickDetector.Detector:FireServer()
           wait()
       end
   end
   
   for i,v in pairs(game:GetService("Workspace").Ovens:GetChildren()) do
       if v.IsOpen.Value then
           v.Door.ClickDetector.Detector:FireServer()
           wait()
       end
   end
   
   for i,v in pairs(game:GetService("Workspace").AllDough:GetChildren()) do
       i=math.fmod(i,8)+1
       Change(v,"CFrame",OvenPositions[i])
       wait()
   end
end
end)
end) if not s then print(f)end
