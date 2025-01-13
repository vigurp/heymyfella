local Camera = workspace.CurrentCamera
local Player = game:GetService("Players").LocalPlayer
local RS = game:GetService("RunService")

local ESP = {}
function ESP:add(object, name, col)
    local namedrawing = Drawing.new("Text")
    namedrawing.Text = name
    namedrawing.Size = 16
    namedrawing.Color = col
    namedrawing.Center = true
    namedrawing.Visible = true
    namedrawing.Transparency = 1
    namedrawing.Position = Vector2.new(0, 0)
    namedrawing.Outline = true
    namedrawing.OutlineColor = Color3.fromRGB(10, 10, 10)
    namedrawing.Font = 3

    local function Update()
        local c
        c = RS.RenderStepped:Connect(function()
            if object.Parent ~= nil and object:FindFirstChild("Pick_Demon_Flower_Thing") then
                local p, vis = Camera:WorldToViewportPoint(object.Position)
                if vis then
                    namedrawing.Position = Vector2.new(p.X, p.Y)
                    namedrawing.Visible = true
                else
                    namedrawing.Visible = false
                end
            else
                namedrawing.Visible = false
                if object.Parent == nil  or not object:FindFirstChild("Pick_Demon_Flower_Thing") then
                    namedrawing:Remove()
                    c:Disconnect()
                end
            end
        end)
    end
    coroutine.wrap(Update)()
end

RS.Heartbeat:Connect(function()
    for _,v in pairs(game:GetService("Workspace")["Demon_Flowers_Spawn"]:GetDescendants()) do
        if v:FindFirstChild('Pick_Demon_Flower_Thing') then
            ESP:add(v, "Lilly Flower", Color3.fromRGB(13, 105, 172))
        end
    end
end)
