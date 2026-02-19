-- @lightgumballhaha (Optimized for 4 accounts)
if not game:IsLoaded() then game.Loaded:Wait() end
getgenv().Configs = {["Quest"] = {["Evo Race V1"] = true, ["Evo Race V2"] = true, ["RGB Haki"] = true, ["Pull Lerver"] = true}, ["FPS Booster"] = true}

local p, rs, ws = game.Players.LocalPlayer, game:GetService("ReplicatedStorage"), workspace
local function join()
    if p.PlayerGui:FindFirstChild("Main (minimal)") then
        rs.Remotes.CommF_:InvokeServer("SetTeam", "Pirates")
    end
end
task.spawn(join)

-- FPS & Lag Fix
if getgenv().Configs["FPS Booster"] then
    settings().Rendering.QualityLevel = 1
    settings().Rendering.GraphicsMode = "NoGraphics"
    for _, v in pairs(game:GetDescendants()) do
        if v:IsA("BasePart") then v.Material = "Plastic" v.CastShadow = false
        elseif v:IsA("Decal") or v:IsA("Texture") then v.Transparency = 1 
        elseif v:IsA("ParticleEmitter") or v:IsA("Trail") then v.Enabled = false end
    end
end

-- Fast Farm Logic
function GetQuest()
    local lv = p.Data.Level.Value
    if lv < 10 then return "Bandit", "BanditQuest1", 1, CFrame.new(1059, 15, 1550)
    elseif lv < 90 then return "Shanda", "SkyExp1Quest", 2, CFrame.new(-7859, 5544, -381)
    elseif lv < 120 then return "Snowman", "SnowQuest", 2, CFrame.new(1389, 86, -1298)
    else return "Prisoner", "PrisonerQuest", 1, CFrame.new(5308, 1, 475) end
end

task.spawn(function()
    while task.wait() do
        local name, qName, qId, qPos = GetQuest()
        if not p.PlayerGui.Main.Quest.Visible then
            p.Character.HumanoidRootPart.CFrame = qPos
            rs.Remotes.CommF_:InvokeServer("StartQuest", qName, qId)
        else
            for _, e in pairs(ws.Enemies:GetChildren()) do
                if e.Name == name and e:FindFirstChild("HumanoidRootPart") then
                    p.Character.HumanoidRootPart.CFrame = e.HumanoidRootPart.CFrame * CFrame.new(0, 20, 0)
                    rs.Remotes.CommF_:InvokeServer("Attack", e)
                end
            end
        end
    end
end)
