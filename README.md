
loadstring(game:HttpGet(("https://raw.githubusercontent.com/ghali201208/-d-/refs/heads/main/README.md")))()
MakeWindow({
  Hub = {
    Title = "-F4M | سكربت المطور محمد",
    Animation = "Powered by F4M"
  },
  Key = {
    KeySystem = false,
    Title = " Key System",
    Description = "",
    KeyLink = "",
    Keys = {"1234"},
    Notifi = {
      Notifications = true,
      CorrectKey = "✅ Running the Script...",
      Incorrectkey = "❌ The key is incorrect!",
      CopyKeyLink = "🔗 Key Link Copied!"
    }
  }
})

MinimizeButton({
  Image = "rbxassetid://98703979045154",
  Size = {50, 50},
  Color = Color3.fromRGB(30, 30, 200),
  Corner = true,
  Stroke = true,
  StrokeColor = Color3.fromRGB(255, 255, 255)
})

-- Main Tab
local Main = MakeTab({
    Name = " المعلومات",
    Image = "rbxassetid://10723415903",
    TabTitle = false
})

-- Header Image with a welcome text
local Image = AddImageLabel(Main, {
  Name = "welcome back",
  Image = "rbxassetid://98703979045154",
  Size = {150, 150},
  Corner = true,
  Stroke = true,
  StrokeColor = Color3.fromRGB(255, 255, 255),
  Shadow = true
})

-- Add a nice label under the image
AddParagraph(Main, {
  Title = "👋 أهلاً بك من جديد!",
  Content = "تم تحميل السكربت بنجاح.\nاستمتع بالتجربة الجديدة المصممة خصيصاً لك 🎉"
})



AddButton(Main, {
  Name = "اضغط لنسخ حسابي تيك",
  Callback = function()
   setclipboard("m_.y.7") 
  end
})

AddButton(Main, {
  Name = "اضغط لنسخ حسابي روب",
  Callback = function()
   setclipboard("Ngfxngfx59") 
  end
})


local Main = MakeTab({
    Name = "أخرى", -- إطار بالنصوص
    Image = "rbxassetid://10734949856",
    TabTitle = false
})

local NotifyToggle = false -- 🚨 حالة التبديل


-- Create a label to show the number of players
local playerCountLabel = AddTextLabel(Main, "الاعبين في السيرفر  " .. #game.Players:GetPlayers())

-- Função para atualizar o número de jogadores quando alguém entra ou sai
local function updatePlayerCount()
    playerCountLabel.Text = "الاعبين في السيرفر: " .. #game.Players:GetPlayers()
end

-- Conecta a função ao evento de entrada de novos jogadores
game.Players.PlayerAdded:Connect(updatePlayerCount)

-- Conecta a função ao evento de saída de jogadores
game.Players.PlayerRemoving:Connect(updatePlayerCount)

-- Atualiza a contagem de jogadores no início (caso tenha jogadores ao carregar o script)
updatePlayerCount()



AddToggle(Main, {
    Name = " تنبيهات دخول/خروج اللاعبين",
    Default = false,
    Callback = function(state)
        NotifyToggle = state
        if NotifyToggle then
            MakeNotifi({
                Title = " مفعّل",
                Text = "سيتم إعلامك عند دخول أو خروج أي لاعب.",
                Time = 3
            })
        else
            MakeNotifi({
                Title = " متوقف",
                Text = "تم إيقاف تنبيهات دخول/خروج اللاعبين.",
                Time = 3
            })
        end
    end
})

-- 🕵️‍♂️ متابعة اللاعبين الجدد
game.Players.PlayerAdded:Connect(function(player)
    if NotifyToggle then
        MakeNotifi({
            Title = " لاعب دخل",
            Text = player.Name .. " انضم إلى السيرفر.",
            Time = 4
        })
    end
end)

-- 🕵️‍♂️ متابعة اللاعبين الي يطلعون
game.Players.PlayerRemoving:Connect(function(player)
    if NotifyToggle then
        MakeNotifi({
            Title = " لاعب خرج",
            Text = player.Name .. " خرج من السيرفر.",
            Time = 4
        })
    end
end)


AddButton(Main, {
    Name = "ادخل سيرفر جديد",
    Callback = function()
        local HttpService = game:GetService("HttpService")
        local success, servers = pcall(function()
            return HttpService:JSONDecode(game:HttpGet("https://games.roblox.com/v1/games/" .. game.PlaceId .. "/servers/Public?sortOrder=Asc&limit=100"))
        end)

        if success and servers and servers.data then
            for _, server in pairs(servers.data) do
                if server.playing < server.maxPlayers and server.id ~= game.JobId then
                    game:GetService("TeleportService"):TeleportToPlaceInstance(game.PlaceId, server.id, game.Players.LocalPlayer)
                    break
                end
            end
        else
            local Sound = Instance.new("Sound", game:GetService("SoundService"))
            Sound.SoundId = "rbxassetid://8183296024"
            Sound:Play()
            game.StarterGui:SetCore("SendNotification", {
                Title = "",
                Text = "فشل العثور على سيرفر ",
                Duration = 3,
                Icon = "rbxassetid://98703979045154"
            })
        end
    end
})

AddButton(Main, {
    Name = "ادخل سيرفر  فارغ",
    Callback = function()
        local HttpService = game:GetService("HttpService")
        local success, servers = pcall(function()
            return HttpService:JSONDecode(game:HttpGet("https://games.roblox.com/v1/games/" .. game.PlaceId .. "/servers/Public?sortOrder=Asc&limit=100"))
        end)

        if success and servers and servers.data then
            for _, server in pairs(servers.data) do
                if server.playing <= 5 then
                    game:GetService("TeleportService"):TeleportToPlaceInstance(game.PlaceId, server.id, game.Players.LocalPlayer)
                    break
                end
            end
        else
            local Sound = Instance.new("Sound", game:GetService("SoundService"))
            Sound.SoundId = "rbxassetid://8183296024"
            Sound:Play()
            game.StarterGui:SetCore("SendNotification", {
                Title = "",
                Text = "فش‌ل الع‌ثور على سيرفر",
                Duration = 3,
                Icon = "rbxassetid://98703979045154"
            })
        end
    end
})

AddButton(Main, {
    Name = "انسخ رقم سيرفرك",
    Callback = function()
        if setclipboard then
            setclipboard(game.JobId)
            local Sound = Instance.new("Sound", game:GetService("SoundService"))
            Sound.SoundId = "rbxassetid://8183296024"
            Sound:Play()
            game.StarterGui:SetCore("SendNotification", {
                Title = "zxe huh",
                Text = "تِمْ النَّسْخ",
                Duration = 3,
                Icon = "rbxassetid://98703979045154"
            })
        else
            local Sound = Instance.new("Sound", game:GetService("SoundService"))
            Sound.SoundId = "rbxassetid://8183296024"
            Sound:Play()
            game.StarterGui:SetCore("SendNotification", {
                Title = "zxe huh",
                Text = "لَمْ يَتِمْ النَّسْخ بِسَبَب حَدُوث خَطَأ",
                Duration = 3,
                Icon = "rbxassetid://98703979045154"
            })
        end
    end
})


local section = AddSection(Main, {"متنوع"})


AddButton(Main, {
    Name = "أخـذ كنـبه",
    Description = "",
    Callback = function()
        local args = {
            [1] = "PickingTools",
            [2] = "Couch"
        }
        game:GetService("ReplicatedStorage").RE:FindFirstChild("1Too1l"):InvokeServer(unpack(args))
    end
})

AddButton(Main, {
    Name = "احـذف الاشـياء الي بـيدك",
    Description = "",
    Callback = function()
        local args = {
            [1] = "ClearAllTools"
        }
        game:GetService("ReplicatedStorage").RE:FindFirstChild("1Clea1rTool1s"):FireServer(unpack(args))
    end
})

AddButton(Main, {
    Name = "انـزع مـلابسك",
    Description = "",
    Callback = function()
        local args = {
            [1] = 2248242028
        }
        game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
        wait(0.5)
        args = {
            [1] = 2547392639
        }
        game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
        wait(1)
        args = {
            [1] = 2248242028
        }
        game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
        wait(0.5)
        args = {
            [1] = 2547392639
        }
        game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(unpack(args))
    end
})

local Main = MakeTab({
    Name = "سكربتات", -- إطار بالنصوص
    Image = "rbxassetid://10723374759",
    TabTitle = false
})

AddButton(Main, {
  Name = "طيران ghali",
  Callback = function()
     loadstring(game:HttpGet("https://pastebin.com/raw/tHAwduZM"))()
  end
})

AddButton(Main, {
  Name = "طيران سياره",
  Callback = function()
     loadstring(game:HttpGet("https://scriptblox.com/raw/Universal-Script-Fly-Car-Mobile-gui-11884"))
  end
})

AddButton(Main, {
  Name = "سكربت الرقصات",
  Callback = function()
     loadstring(game:HttpGet("https://pastebin.com/raw/cR28U1y1"))
  end
})

AddButton(Main, {
  Name = "سكربت الفا",
  Callback = function()
    loadstring(game:HttpGet("https://rawscripts.net/raw/Universal-Script-ALFA-IRAQ-34036"))()
  end
})

AddButton(Main, {
    Name = "سكربت جودة  ",
    Callback = function()
      loadstring(game:HttpGet("https://rawscripts.net/raw/Universal-Script-Rtx-graphics-25102"))()
    end
  })

AddButton(Main, {
    Name = "مشيات ",
    Callback = function()
      loadstring(game:HttpGet("https://rawscripts.net/raw/Universal-Script-AFEM-14048"))()
    end
  })

AddButton(Main, {
    Name = "سكربت نسخ سكنـات ",
    Callback = function()
      loadstring(game:HttpGet("https://rawscripts.net/raw/Universal-Script-rochips-universal-18294"))()
    end
  })
  
  AddButton(Main, {
    Name = "سكربت الهلال ",
    Callback = function()
      loadstring(game:HttpGet("https://raw.githubusercontent.com/n0kc/AtomicHub/main/Map-Al-Biout.lua"))()
    end
  })
  
  AddButton(Main, {
    Name = "سكربت اختفاء ",
    Callback = function()
      loadstring(game:HttpGet('https://pastebin.com/raw/3Rnd9rHf'))()
    end
  })
  
  AddButton(Main, {
    Name = " سكربت طيران كنبة  ",
    Callback = function()
      loadstring(game:HttpGet("https://raw.githubusercontent.com/0Ben1/fe./main/Fling%20GUI"))()
    end
  })
  
  AddButton(Main, {
    Name = " سكربـت اغاني  ",
    Callback = function()
      loadstring(game:HttpGet('https://raw.githubusercontent.com/M1ZZ001/BrookhavenR4D/main/Brookhaven%20R4D%20Script'))()
    end
  })

  AddButton(Main, {
    Name = "سكربت قفل",
    Callback = function()
      loadstring(game:HttpGet("https://raw.githubusercontent.com/Sector9922/SECTOR-SHIFT-LOCK/main/SECTOR%20SHIFT%20LOCK"))()
    end
  })


local Main = MakeTab({
    Name = "الاعب", -- إطار بالنصوص
    Image = "rbxassetid://10747373176",
    TabTitle = false
})

AddButton(Main, {
  Name = "طيران",
  Callback = function()
   loadstring(game:HttpGet("https://pastebin.com/raw/tHAwduZM"))() 
  end
})

local WalkSpeedEnabled = false
local WalkSpeedValue = 16

AddTextBox(Main, {
    Name = "سرعة ",
    Default = "",
    PlaceholderText = "هــنــا",
    ClearText = true,
    Callback = function(value)
        WalkSpeedValue = tonumber(value)
        if WalkSpeedEnabled then
            game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = WalkSpeedValue
        end
    end
})

AddToggle(Main, {
    Name = "تفعيل السرعه",
    Default = false,
    Callback = function(Value)
        WalkSpeedEnabled = Value
        if Value then
            game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = WalkSpeedValue
        else
            game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = 16 -- القيمة الافتراضية
        end
    end
})

local JumpPowerEnabled = false
local JumpPowerValue = 50

AddTextBox(Main, {
    Name = "قفز ",
    Default = "",
    PlaceholderText = "هــنــا",
    ClearText = true,
    Callback = function(value)
        JumpPowerValue = tonumber(value)
        if JumpPowerEnabled then
            game.Players.LocalPlayer.Character.Humanoid.JumpPower = JumpPowerValue
        end
    end
})

AddToggle(Main, {
    Name = "تفعيل القفز",
    Default = false,
    Callback = function(Value)
        JumpPowerEnabled = Value
        if Value then
            game.Players.LocalPlayer.Character.Humanoid.JumpPower = JumpPowerValue
        else
            game.Players.LocalPlayer.Character.Humanoid.JumpPower = 50 -- القيمة الافتراضية
        end
    end
})

local FovEnabled = false
local FovValue = 70

AddTextBox(Main, {
    Name = "ابعاد الشاشه",
    Default = "",
    PlaceholderText = "هــنــا",
    ClearText = true,
    Callback = function(value)
        FovValue = tonumber(value)
        if FovEnabled then
            workspace.CurrentCamera.FieldOfView = FovValue
        end
    end
})

AddToggle(Main, {
    Name = "تفعيل ابعاد الشاشه",
    Default = false,
    Callback = function(Value)
        FovEnabled = Value
        if Value then
            workspace.CurrentCamera.FieldOfView = FovValue
        else
            workspace.CurrentCamera.FieldOfView = 70
        end
    end
})

local SpinEnabled = false
local Spinspeed = 0
local SpinObj

AddTextBox(Main, {
    Name = " دوران",
    Default = "",
    PlaceholderText = "هــنــا",
    ClearText = true,
    Callback = function(Value)
        Spinspeed = tonumber(Value)
        if SpinEnabled then
            if SpinObj then SpinObj:Destroy() end
            SpinObj = Instance.new("BodyAngularVelocity")
            SpinObj.Parent = game.Players.LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
            SpinObj.MaxTorque = Vector3.new(0, math.huge, 100)
            SpinObj.AngularVelocity = Vector3.new(100, Spinspeed, 0)
        end
    end
})

AddToggle(Main, {
    Name = "تفعيل الدوران",
    Default = false,
    Callback = function(Value)
        SpinEnabled = Value
        if Value then
            if SpinObj then SpinObj:Destroy() end
            SpinObj = Instance.new("BodyAngularVelocity")
            SpinObj.Parent = game.Players.LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
            SpinObj.MaxTorque = Vector3.new(0, math.huge, 100)
            SpinObj.AngularVelocity = Vector3.new(100, Spinspeed, 0)
        else
            if SpinObj then SpinObj:Destroy() end
        end
    end
})

AddButton(Main, {
    Name = "تعليق السيرفر ",
    Callback = function()
        local player = game.Players.LocalPlayer
        local char = player.Character or player.CharacterAdded:Wait()
        local hrp = char:WaitForChild("HumanoidRootPart")
        local humanoid = char:WaitForChild("Humanoid")

        local toolsName = "FireX"
        local toolPart = workspace:FindFirstChild("WorkspaceCom") and workspace.WorkspaceCom:FindFirstChild("001_GiveTools")
        local cleartoolremote = game:GetService("ReplicatedStorage"):FindFirstChild("ClearAllTools") -- تأكد من اسم الريموت

        if not toolPart or not toolPart:FindFirstChild(toolsName) then
            warn("FireX غير موجود في WorkspaceCom/001_GiveTools")
            return
        end

        -- فك الجلوس
        if humanoid.Sit then
            humanoid.Sit = false
        end

        -- حذف الكاميرا
        local cam = workspace:FindFirstChild("Camera")
        if cam then cam:Destroy() end
        wait(0.1)
        if workspace:FindFirstChild("Camera") then
            workspace:FindFirstChild("Camera"):Destroy()
        end

        -- تحريك اللاعب أسفل FireX
        hrp.CFrame = toolPart.FireX.CFrame + Vector3.new(0, -15, 0)
        wait(0.2)
        hrp.Anchored = true
        local ddos = true

        for i = 1, 1350 do
            task.wait()

            if not ddos then
                if cleartoolremote then
                    cleartoolremote:FireServer("ClearAllTools")
                end
                hrp.Anchored = false
                hrp.CFrame = CFrame.new(9999, -475, 9999)
                return
            end

            -- حذف الكاميرا مرة ثانية إن وُجدت
            local cam2 = workspace:FindFirstChild("Camera")
            if cam2 then cam2:Destroy() end

            -- حذف الأداة FireX من الشخصية إن وُجدت
            local tool = char:FindFirstChild(toolsName)
            if tool then tool:Destroy() end

            -- كليك على FireX
            local clickdetector = toolPart.FireX:FindFirstChildOfClass("ClickDetector")
            if clickdetector then
                fireclickdetector(clickdetector)
            end
        end

        -- النهاية
        hrp.Anchored = false
        hrp.CFrame = CFrame.new(0, -475, 0)
    end
})



local UIS = game:GetService("UserInputService")
local Players = game:GetService("Players")
local player = Players.LocalPlayer

local infiniteJumpEnabled = false
local connection

-- دالة لإرسال إشعار
local function MakeNotifi(notification)
    game.StarterGui:SetCore("SendNotification", {
        Title = notification.Title,
        Text = notification.Text,
        Duration = notification.Time or 3
    })
end

-- عند تغيير حالة التوغل
local function onInfiniteJumpToggle(value)
    infiniteJumpEnabled = value

    MakeNotifi({
        Title = "القفز اللانهائي",
        Text = value and "تم التفعيل ✅" or "تم الإيقاف ❌",
        Time = 2
    })

    if not connection then
        connection = UIS.JumpRequest:Connect(function()
            if infiniteJumpEnabled then
                local character = player.Character
                local humanoid = character and character:FindFirstChildOfClass("Humanoid")
                if humanoid then
                    humanoid:ChangeState(Enum.HumanoidStateType.Jumping)
                end
            end
        end)
    end
end

-- زر التوغل في الواجهة
local Toggle = AddToggle(Main, {
    Name = "قفز لا نهائي ",
    Default = false,
    Callback = onInfiniteJumpToggle
})


-- زر لإخفاء اللاعب (يصبح صغير جدًا)
AddButton(Main, {
    Name = "اختفاء اللاعب",
    Callback = function()
        local args = {
            [1] = "CharacterSizeUp",
            [2] = 6 -- حجم مصغّر (كلما زاد الرقم صغر الحجم أكثر)
        }
        game:GetService("ReplicatedStorage").RE:FindFirstChild("1Clothe1s"):FireServer(unpack(args))
    end
})

-- زر لإعادة حجم اللاعب إلى الطبيعي
AddButton(Main, {
    Name = "إلغاء اختفاء",
    Callback = function()
        local args = {
            [1] = "CharacterSizeUp",
            [2] = 1 -- الحجم الطبيعي
        }
        game:GetService("ReplicatedStorage").RE:FindFirstChild("1Clothe1s"):FireServer(unpack(args))
    end
})

-- Variável para controlar o estado do Noclip
local noclipEnabled = false
local runService = game:GetService("RunService")

-- Função para definir CanCollide para todas as partes do personagem
local function setCharacterCanCollide(character, canCollide)
    for _, part in ipairs(character:GetDescendants()) do
        if part:IsA("BasePart") then
            part.CanCollide = canCollide
        end
    end
end

-- Função de callback para o botão de alternância de Noclip
local function onNoclipToggle(value)
    noclipEnabled = value
    print("Noclip Enabled:", noclipEnabled)
    
    local player = game.Players.LocalPlayer
    local character = player.Character

    if noclipEnabled then
        -- Inicia um loop para continuamente definir CanCollide
        noclipLoop = runService.Stepped:Connect(function()
            if character then
                setCharacterCanCollide(character, false)
            end
        end)
    else
        -- Desativa o loop e restaura CanCollide
        if noclipLoop then
            noclipLoop:Disconnect()
        end
        if character then
            setCharacterCanCollide(character, true)
        end
    end
end

-- Adiciona o botão de alternância "Noclip"
local Toggle = AddToggle(Main, {
    Name = " اخترق الجدار",
    Default = false,
    Callback = onNoclipToggle
})


local Main = MakeTab({
    Name = "الافتار", -- إطار بالنصوص
    Image = "rbxassetid://10734952036",
    TabTitle = false
})

local section = AddSection(Main, {"ورد"})


AddButton(Main, {
Name = "ازالة الملابس لضمان جوده افضل",
Callback = function()
--[[
	WARNING: Heads up! This script has not been verified by ScriptBlox. Use at your own risk!
]]
local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()

local function removeClothes()
    for _, item in pairs(character:GetChildren()) do
        if item:IsA("Shirt") or item:IsA("Pants") then
            item:Destroy()
        end
    end
end

removeClothes()
end
})

--[[
  Name = "Main" <string> Nome da guia
]]
AddSection(Main, {"نسخ سكنات كامله"})

local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Players = game:GetService("Players")
local StarterGui = game:GetService("StarterGui")

local RemoteWear = ReplicatedStorage:WaitForChild("Remotes"):WaitForChild("Wear")
local RemoteBody = ReplicatedStorage:WaitForChild("Remotes"):WaitForChild("ChangeCharacterBody")

local function Notify(title, text)
    StarterGui:SetCore("SendNotification", {
        Title = title,
        Text = text,
        Duration = 3
    })
end

local function Wear(id)
    if id and id ~= 0 then
        RemoteWear:InvokeServer(id)
    end
end

local function ApplySkinTone(player)
    local char = player.Character or player.CharacterAdded:Wait()
    local bodyColors = char:FindFirstChildOfClass("BodyColors")
    if bodyColors then
        Wear(bodyColors.HeadColor.Name)
    end
end

local function CopyClothing(player)
    local h = (player.Character or player.CharacterAdded:Wait()):FindFirstChildOfClass("Humanoid")
    if h then
        local d = h:GetAppliedDescription()
        for _, id in ipairs({d.Shirt, d.Pants, d.GraphicTShirt}) do
            Wear(id)
            task.wait(0.2)
        end
    end
end

local function CopyAccessories(player)
    local h = (player.Character or player.CharacterAdded:Wait()):FindFirstChildOfClass("Humanoid")
    if h then
        local d = h:GetAppliedDescription()
        local all = {
            d.HatAccessory, d.HairAccessory, d.FaceAccessory,
            d.NeckAccessory, d.ShouldersAccessory, d.FrontAccessory,
            d.BackAccessory, d.WaistAccessory
        }
        for _, accList in ipairs(all) do
            if accList then
                for id in string.gmatch(accList, "%d+") do
                    Wear(tonumber(id))
                    task.wait(0.2)
                end
            end
        end
    end
end

local function CopyBodyParts(player)
    local h = (player.Character or player.CharacterAdded:Wait()):FindFirstChildOfClass("Humanoid")
    if h then
        local d = h:GetAppliedDescription()
        local bodyIds = {
            d.Torso, d.RightArm, d.LeftArm,
            d.RightLeg, d.LeftLeg, d.Head
        }
        if RemoteBody then
            RemoteBody:InvokeServer(bodyIds)
        else
            Notify("خطأ", "RemoteBody غير موجود.")
        end
    end
end

local function CopySleeves(player)
    
    local h = (player.Character or player.CharacterAdded:Wait()):FindFirstChildOfClass("Humanoid")
    if h then
        local d = h:GetAppliedDescription()
        local sleeveIds = {
            d.LeftArm, d.RightArm 
        }

        for _, id in ipairs(sleeveIds) do
            if id then
                Wear(id) 
                task.wait(0.2)
            end
        end
    end
end

local selectedPlayer = nil

local function GetPlayerList()
    local list = {}
    for _, p in ipairs(Players:GetPlayers()) do
        if p ~= Players.LocalPlayer then
            table.insert(list, p.Name)
        end
    end
    return list
end

AddDropdown(Main, {
    Name = "اختار اللاعب",
    Default = "",
    Options = GetPlayerList(),
    Callback = function(value)
        selectedPlayer = value
    end
})

AddButton(Main, {
    Name = "نسخ السكن",
    Callback = function()
        if not selectedPlayer then
            Notify("خطأ", "يجب اختيار لاعب أولاً")
            return
        end

        local player = Players:FindFirstChild(selectedPlayer)  
        if not player then return end  

        Notify("جاري النسخ", "جاري نسخ سكن " .. player.Name)  

        task.spawn(function()  
            
            CopyBodyParts(player)  
            task.wait(0.3)  
            ApplySkinTone(player)  
            task.wait(0.3)  
            CopyClothing(player)  
            task.wait(0.3)  
            CopyAccessories(player)  
            task.wait(0.3)  
            CopySleeves(player) 
            Notify("تم النسخ", "تم نسخ سكن " .. player.Name .. " بنجاح!")  
        end)  
    end
})


AddDropdown(Main, {
    Name = "اختر نوع الورد",
    Default = "اختر من هنا",
    Options = {"ورد اسود", "ورد احمر", "ورد ابيض", "ورد وردي"},
    Callback = function(value)
        selectedFlower = value
    end
})

AddButton(Main, {
    Name = "تطبيق الورد",
    Callback = function()
        local ids = {
            ["ورد اسود"] = 12465465333,
            ["ورد احمر"] = 86738633187728,
            ["ورد ابيض"] = 72175664063418,
            ["ورد وردي"] = 12465478536
        }

        if selectedFlower and ids[selectedFlower] then
            game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(ids[selectedFlower])
        else
            warn("لم يتم اختيار أي نوع من الورد")
        end
    end
})


local section = AddSection(Main, {"احذيه"})

AddDropdown(Main, {
    Name = "اختر نوع الحذاء",
    Default = "اختر من هنا",
    Options = {"حذاء اسود", "حذاء ابيض", "حذاء وردي", "حذاء اسود احمر", "حذاء احمر واسود"},
    Callback = function(value)
        selectedShoe = value
    end
})

AddButton(Main, {
    Name = "تطبيق الحذاء",
    Callback = function()
        local ids = {
            ["حذاء اسود"] = 14388004031,
            ["حذاء ابيض"] = 14388009243,
            ["حذاء وردي"] = 14388006902,
            ["حذاء اسود احمر"] = 14388001192,
            ["حذاء احمر واسود"] = 14388019333
        }

        if selectedShoe and ids[selectedShoe] then
            game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(ids[selectedShoe])
        else
            warn("لم يتم اختيار أي نوع من الأحذية")
        end
    end
})


local section = AddSection(Main, {"قفازات طويله"})

AddDropdown(Main, {
    Name = "اختر نوع القفاز الطويل",
    Default = "اختر من هنا",
    Options = {"قفاز طويل اسود", "قفاز طويل احمر", "قفاز طويل ابيض", "قفاز طويل ازرق", "قفاز طويل وردي", "قفاز طويل أخضر"},
    Callback = function(value)
        selectedGlove = value
    end
})

AddButton(Main, {
    Name = "تطبيق القفاز الطويل",
    Callback = function()
        local ids = {
            ["قفاز طويل اسود"] = 10789914680,
            ["قفاز طويل احمر"] = 15209194774,
            ["قفاز طويل ابيض"] = 10789933479,
            ["قفاز طويل ازرق"] = 10789945803,
            ["قفاز طويل وردي"] = 10789939838,
            ["قفاز طويل أخضر"] = 13233318125,
        }

        if selectedGlove and ids[selectedGlove] then
            game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(ids[selectedGlove])
        else
            warn("لم يتم اختيار أي نوع من القفازات")
        end
    end
})



local section = AddSection(Main, {"أشواك في الجسم"})
AddDropdown(Main, {
    Name = "اختر نوع الشوك",
    Default = "اختر من هنا",
    Options = {"شوك اسود", "شوك ابيض", "شوك اسود في جميع أجزاء الجسم"},
    Callback = function(value)
        selectedSpike = value
    end
})

AddButton(Main, {
    Name = "تطبيق الشوك",
    Callback = function()
        local ids = {
            ["شوك اسود"] = 17406577951,
            ["شوك ابيض"] = 17406634097,
            ["شوك اسود في جميع أجزاء الجسم"] = 13463285148,
        }

        if selectedSpike and ids[selectedSpike] then
            game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(ids[selectedSpike])
        else
            warn("لم يتم اختيار نوع الشوك")
        end
    end
})

local section = AddSection(Main, {"جواكيت أولاد"})


AddDropdown(Main, {
    Name = "اختر الجاكيت",
    Default = "اختر من هنا",
    Options = {"جاكيت اسود", "جاكيت اسود مزخرف ابيض"},
    Callback = function(value)
        selectedJacket = value
    end
})

AddButton(Main, {
    Name = "تطبيق الجاكيت",
    Callback = function()
        local ids = {
            ["جاكيت اسود"] = 9048321833,
            ["جاكيت اسود مزخرف ابيض"] = 15154273975,
        }

        if selectedJacket and ids[selectedJacket] then
            game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(ids[selectedJacket])
        else
            warn("لم يتم اختيار جاكيت")
        end
    end
})


local section = AddSection(Main, {"جواكيت بنات"})


AddDropdown(Main, {
    Name = "اختر الجاكيت",
    Default = "اختر من هنا",
    Options = {"جاكيت اسود", "جاكيت ابيض"},
    Callback = function(value)
        selectedGirlJacket = value
    end
})

AddButton(Main, {
    Name = "تطبيق الجاكيت",
    Callback = function()
        local ids = {
            ["جاكيت اسود"] = 14900095685,
            ["جاكيت ابيض"] = 14849843673,
        }

        if selectedGirlJacket and ids[selectedGirlJacket] then
            game:GetService("ReplicatedStorage").Remotes.Wear:InvokeServer(ids[selectedGirlJacket])
        else
            warn("لم يتم اختيار جاكيت")
        end
    end
})


local Main = MakeTab({
    Name = "الاجسام", -- إطار بالنصو
    Image = "rbxassetid://17465429968",
    TabTitle = false
})

local section = AddSection(Main, {"أجسام أولاد"})


AddDropdown(Main, {
    Name = "اختر الجسم",
    Default = "اختر من هنا",
    Options = {
        "جسم ولد نحيف 1",
        "جسم ولد نحيف 2",
        "جسم ولد كوبي",
        "جسم ولد رول"
    },
    Callback = function(value)
        selectedBoyBody = value
    end
})

AddButton(Main, {
    Name = "تطبيق الجسم",
    Callback = function()
        local bodies = {
            ["جسم ولد نحيف 1"] = {17754346388, 1, 1, 1, 1, 1},
            ["جسم ولد نحيف 2"] = {92757812011061, 99519402284266, 115905570886697, 1, 1, 1},
            ["جسم ولد كوبي"] = {86499666, 27112039, 27112052, 27112068, 27112056, 1},
            ["جسم ولد رول"] = {27112025, 27112039, 27112052, 3230472745, 3230470862, 1}
        }

        if selectedBoyBody and bodies[selectedBoyBody] then
            game:GetService("ReplicatedStorage").Remotes.ChangeCharacterBody:InvokeServer(bodies[selectedBoyBody])
        else
            warn("لم يتم اختيار جسم")
        end
    end
})


local section = AddSection(Main, {"أجسام بنات"})


AddDropdown(Main, {
    Name = "اختر الجسم",
    Default = "اختر من هنا",
    Options = {
        "جسم بنت 1",
        "جسم بنت 2",
        "جسم بنت 3",
        "جسم بنت 4",
        "جسم بنت 5",
        "جسم بنت 6",
        "جسم بنت 7"
    },
    Callback = function(value)
        selectedGirlBody = value
    end
})

AddButton(Main, {
    Name = "تطبيق الجسم",
    Callback = function()
        local bodies = {
            ["جسم بنت 1"] = {96491916349570, 76683091425509, 75159926897589, 1, 1, 1},
            ["جسم بنت 2"] = {96491916349570, 14854350570, 14854350451, 1, 1, 1},
            ["جسم بنت 3"] = {74302534603111, 76683091425509, 75159926897589, 1, 1, 1},
            ["جسم بنت 4"] = {16214246112, 76683091425509, 16214251181, 1, 1, 1},
            ["جسم بنت 5"] = {124754866635882, 76683091425509, 75159926897589, 1, 1, 1},
            ["جسم بنت 6"] = {18839824113, 18839824209, 18839824132, 1, 1, 1},
            ["جسم بنت 7"] = {15539008532, 15539008875, 15539008680, 15539008795, 15539011945, 1},
        }

        if selectedGirlBody and bodies[selectedGirlBody] then
            game:GetService("ReplicatedStorage").Remotes.ChangeCharacterBody:InvokeServer(bodies[selectedGirlBody])
        else
            warn("لم يتم اختيار جسم")
        end
    end
})

local section = AddSection(Main, {"أجسام أقزام"})


AddDropdown(Main, {
    Name = "اختر جسم القزم",
    Default = "اختر من هنا",
    Options = {
        "جسم قزم عادي",
        "جسم قزم متوسط",
        "جسم قزم صغيره [ كيوت ]"
    },
    Callback = function(value)
        selectedDwarfBody = value
    end
})

AddButton(Main, {
    Name = "تطبيق الجسم",
    Callback = function()
        local bodies = {
            ["جسم قزم عادي"] = {14579958702, 14579959062, 14579959191, 14579959249, 14579963667, 1},
            ["جسم قزم متوسط"] = {77813057823038, 135110043370135, 116607813654019, 138966229804486, 128961183894053, 1},
            ["جسم قزم صغيره [ كيوت ]"] = {120973199097564, 118345433416023, 112849465115864, 78321005147549, 106586789635639, 1}
        }

        if selectedDwarfBody and bodies[selectedDwarfBody] then
            game:GetService("ReplicatedStorage").Remotes.ChangeCharacterBody:InvokeServer(bodies[selectedDwarfBody])
        else
            warn("لم يتم اختيار جسم")
        end
    end
})


local section = AddSection(Main, {"أجسام غريبة"})

AddDropdown(Main, {
    Name = "اختر الجسم الغريب",
    Default = "اختر من هنا",
    Options = {
        "جــســم هــامــســتــر",
        "جــســم تــرولــكــس",
        "روبــوت كــبــيــر",
        "جــســم هــولــك",
        "جــســم كــرســي",
        "جــســم وجــه مــبــتــســم",
        "جــســم بــطــة",
        "جــســم مــعــضــل",
        "جــســم عــودة ابــيــض",
        "جــســم قــيــتــار",
        "جــســم بــطــاطــا",
        "جــســم فرن",
        "جــســم فــيــنــوم",
        "جــســم صــرصــور"
    },
    Callback = function(value)
        selectedWeirdBody = value
    end
})

AddButton(Main, {
    Name = "تطبيق الجسم الغريب",
    Callback = function()
        local bodies = {
            ["جــســم هــامــســتــر"] = {14898536974, 14898536957, 14898537277, 14898537300, 14898537292, 14898536975},
            ["جــســم تــرولــكــس"] = {18369636653, 18369636748, 18369636830, 18369636826, 18369636724, 18369636293},
            ["روبــوت كــبــيــر"] = {14776593226, 14776227941, 14776227816, 102149844389538, 102624006545764, 74920391713702},
            ["جــســم هــولــك"] = {105938035513300, 120682289281525, 78459091342559, 119167161940457, 78171925423915, 92193892051712},
            ["جــســم كــرســي"] = {16872133248, 16872133982, 16872133723, 16872133730, 16872133733, 134082579},
            ["جــســم وجــه مــبــتــســم"] = {14711444370, 14711444605, 14711444625, 14711444360, 14711444358, 14711444241},
            ["جــســم بــطــة"] = {18324039722, 18324039646, 18324039738, 18324039702, 18324039831, 18324039660},
            ["جــســم مــعــضــل"] = {15051038989, 15051041997, 15051039303, 15051039296, 15051039336, 15051039044},
            ["جــســم عــودة ابــيــض"] = {18336954738, 18336954554, 18336954464, 18336954818, 18336954617, 18336954428},
            ["جــســم قــيــتــار"] = {108643840693910, 107517968737204, 112649567959319, 84616648133636, 139750854834352, 133464387421778},
            ["جــســم بــطــاطــا"] = {18574419908, 18574420035, 18574420049, 18574419752, 18574421837, 18574419869},
            ["جــســم فرن"] = {86447020764936, 139530639863319, 139698687689285, 83162413572492, 110551097250943, 86740411151772},
            ["جــســم فــيــنــوم"] = {17468911983, 17468912889, 17468912765, 17468911990, 17468910484, 17468910274},
            ["جــســم صــرصــور"] = {17463303942, 17463303907, 17463304004, 17463303921, 17463307062, 17463303991}
        }

        if selectedWeirdBody and bodies[selectedWeirdBody] then
            game:GetService("ReplicatedStorage").Remotes.ChangeCharacterBody:InvokeServer(bodies[selectedWeirdBody])
        else
            warn("لم يتم اختيار جسم")
        end
    end
})


local Main = MakeTab({
    Name = "المتعه",
    Image = "rbxassetid://131153193945220",
    TabTitle = false
})

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer

local playerNames = {}
for _, plr in ipairs(Players:GetPlayers()) do
    if plr ~= LocalPlayer then
        table.insert(playerNames, plr.Name)
    end
end

local selectedPlayerName = nil

local Dropdown = AddDropdown(Main, {
    Name = "اختر لاعب",
    Options = playerNames,
    Default = playerNames[1],
    Callback = function(Value)
        selectedPlayerName = Value
    end
})

AddButton(Main, {
    Name = "قتل بالسفينه بلوتوث",
    Callback = function()
        if not selectedPlayerName then
            warn("لم يتم اختيار لاعب")
            return
        end

        MakeNotifi({
            Title = "تم التشغيل",
            Text = "لا تفعل الامر اكثر من مرا",
            Time = 5
        })

        local targetPlayer = Players:FindFirstChild(selectedPlayerName)
        if targetPlayer and targetPlayer.Character and targetPlayer.Character:FindFirstChild("HumanoidRootPart") then
            local character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
            local humanoidRootPart = character:WaitForChild("HumanoidRootPart")

            local originalPosition = humanoidRootPart.Position
            local originalAnchoredState = humanoidRootPart.Anchored

            -- ننقل اللاعب لمنطقة تشغيل السفينة
            humanoidRootPart.CFrame = CFrame.new(634.18, -4.00, 1839.65)
            wait(0.5)

            -- تشغيل السفينة (لا تخلي اللاعب يركبها)
            local args = {
                "PickingBoat",
                "MilitaryBoatFree"
            }
            game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1Ca1r"):FireServer(unpack(args))
            wait(1.5)

            -- نحرك السفينة فوق لاعب الهدف مباشرة بدون ركوب
            local targetPosition = targetPlayer.Character.HumanoidRootPart.Position
            local vehicle = workspace.Vehicles:FindFirstChild(LocalPlayer.Name .. "Car")

            if vehicle then
                -- ننقل السفينة لمكان اللاعب الهدف مباشرة (ممكن تسبب ضرر)
                vehicle:SetPrimaryPartCFrame(CFrame.new(targetPosition + Vector3.new(0, -2, 0)))

                -- نرفع شخصية اللاعب عندك بعيداً لتجنب تداخل السفينة معك
                humanoidRootPart.CFrame = CFrame.new(targetPosition + Vector3.new(0, 50, 0))

                -- ممكن تحرك السفينة بشكل عشوائي لفترة لتأثير القتل
                local crazyStart = tick()
                while tick() - crazyStart < 2 do
                    local offset = Vector3.new(
                        math.random(-25, 12),
                        math.random(-13, 10),
                        math.random(-10, 18)
                    )
                    vehicle:SetPrimaryPartCFrame(CFrame.new(targetPosition + Vector3.new(0, -2, 0) + offset))
                    wait(0.05)
                end
            end

            -- إزالة السفينة من العالم
            if vehicle then
                vehicle:Destroy()
            end

            -- إرجاع اللاعب لموقعه الأصلي
            wait(0.5)
            humanoidRootPart.CFrame = CFrame.new(originalPosition)
            humanoidRootPart.Anchored = originalAnchoredState

            local finalArgs = {
                [1] = "PickingBoat",
                [2] = "MilitaryBoatFree"
            }
            game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1Ca1r"):FireServer(unpack(finalArgs))

            wait(0.5)
            local deleteArgs = {
                [1] = "DeleteAllVehicles"
            }
            game:GetService("ReplicatedStorage").RE:FindFirstChild("1Ca1r"):FireServer(unpack(deleteArgs))
        else
            warn("اللاعب غير موجود أو لا يملك الشخصية")
        end
    end

})

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer

local playerNames = {}
for _, plr in ipairs(Players:GetPlayers()) do
    if plr ~= LocalPlayer then
        table.insert(playerNames, plr.Name)
    end
end

local selectedPlayerName = nil

local Dropdown = AddDropdown(Main, {
    Name = "اختر لاعب",
    Options = playerNames,
    Default = playerNames[1],
    Callback = function(Value)
        selectedPlayerName = Value
    end
})

AddButton(Main, {
    Name = "قتل بالسفينه عادي",
    Callback = function()
        if not selectedPlayerName then
            warn("لم يتم اختيار لاعب")
            return
        end

        MakeNotifi({
            Title = "تم التشغيل",
            Text = "لا تفعله اكثر من مرا",
            Time = 5
        })

        local targetPlayer = Players:FindFirstChild(selectedPlayerName)
        if targetPlayer and targetPlayer.Character and targetPlayer.Character:FindFirstChild("HumanoidRootPart") then
            local character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
            local humanoid = character:WaitForChild("Humanoid")
            local humanoidRootPart = character:WaitForChild("HumanoidRootPart")

            local originalPosition = humanoidRootPart.Position
            local originalAnchoredState = humanoidRootPart.Anchored

            humanoidRootPart.CFrame = CFrame.new(634.18, -4.00, 1839.65)
            wait(0.5)

            local args = {
                "PickingBoat",
                "PirateFree"
            }
            game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1Ca1r"):FireServer(unpack(args))
            wait(1.5)

            local function sitInBoat()
                local vehicleSeat = workspace.Vehicles:FindFirstChild("doctonbcCar")
                if not vehicleSeat then return end

                vehicleSeat = vehicleSeat.Body:FindFirstChild("VehicleSeat")
                if not vehicleSeat then return end

                humanoidRootPart.Anchored = false
                humanoid:ChangeState(Enum.HumanoidStateType.GettingUp)
                wait(0.2)

                humanoidRootPart.CFrame = vehicleSeat.CFrame * CFrame.new(0, 0.5, 0)
                wait(0.2)

                humanoid.Sit = true
                firetouchinterest(humanoidRootPart, vehicleSeat, 0)
                firetouchinterest(humanoidRootPart, vehicleSeat, 1)
                wait(0.5)

                if humanoid.SeatPart ~= vehicleSeat then
                    humanoidRootPart.CFrame = vehicleSeat.CFrame * CFrame.new(0, 0.5, 0)
                    humanoid.Sit = true
                    wait(0.5)
                end
            end

            sitInBoat()
            wait(0.5)

            local targetPosition = targetPlayer.Character.HumanoidRootPart.Position
            local vehicle = workspace.Vehicles:FindFirstChild(LocalPlayer.Name .. "Car")

            if vehicle then
                vehicle:SetPrimaryPartCFrame(CFrame.new(targetPosition + Vector3.new(0, -2, 0)))
                humanoidRootPart.CFrame = CFrame.new(targetPosition + Vector3.new(0, 5, 0))

                local crazyStart = tick()
                while tick() - crazyStart < 2 do
                    local offset = Vector3.new(
                        math.random(-25, 12),
                        math.random(-13, 10),
                        math.random(-10, 18)
                    )
                    vehicle:SetPrimaryPartCFrame(CFrame.new(targetPosition + Vector3.new(0, -2, 0) + offset))
                    wait(0.05)
                end
            end

            local targetDestination = Vector3.new(-86.00, -224.27, 34.57)
            if vehicle then
                vehicle:SetPrimaryPartCFrame(CFrame.new(targetDestination))
                humanoidRootPart.CFrame = CFrame.new(targetDestination + Vector3.new(0, 5, 0))
            end

            wait(0.5)
            humanoidRootPart.Anchored = false
            humanoid:ChangeState(Enum.HumanoidStateType.GettingUp)

            if vehicle then
                vehicle:Destroy()
            end

            wait(0.5)
            humanoidRootPart.CFrame = CFrame.new(originalPosition)
            humanoidRootPart.Anchored = originalAnchoredState
            humanoid:ChangeState(Enum.HumanoidStateType.RunningNoPhysics)

            local finalArgs = {
                [1] = "PickingBoat",
                [2] = "PirateFree"
            }
            game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1Ca1r"):FireServer(unpack(finalArgs))

            wait(0.5)
            local deleteArgs = {
                [1] = "DeleteAllVehicles"
            }
            game:GetService("ReplicatedStorage").RE:FindFirstChild("1Ca1r"):FireServer(unpack(deleteArgs))
        else
            warn("اللاعب غير موجود أو لا يملك الشخصية")
        end
    end
}) 

local section = AddSection(Main, {"قتل باص"})

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local SelectedPlayerName = nil

local playerNames = {}
for _, player in ipairs(Players:GetPlayers()) do
    if player ~= LocalPlayer then
        table.insert(playerNames, player.Name)
    end
end

local Dropdown = AddDropdown(Main, {
    Name = "اختر لاعب",
    Options = playerNames,
    Default = playerNames[1] or "",
    Callback = function(Value)
        SelectedPlayerName = Value
    end
})

AddButton(Main, {
    Name = "قتل بالباص بلوتوث",
    Callback = function()
        if not SelectedPlayerName then
            warn("اختر لاعبًا أولًا")
            return
        end

        local targetPlayer = Players:FindFirstChild(SelectedPlayerName)
        if not (targetPlayer and targetPlayer.Character and targetPlayer.Character:FindFirstChild("HumanoidRootPart")) then
            warn("اللاعب غير موجود أو لم يُحمّل")
            return
        end

        local character = LocalPlayer.Character or LocalPlayer.CharacterAdded:Wait()
        local humanoidRootPart = character:WaitForChild("HumanoidRootPart")
        local originalPosition = humanoidRootPart.Position

        -- التلغراف لنقطة استدعاء الباص
        humanoidRootPart.CFrame = CFrame.new(1082.86, 76.00, -1125.20)
        wait(0.3)

        -- استدعاء الباص
        local spawnArgs = {
            [1] = "PickingCar",
            [2] = "SchoolBus"
        }
        game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1Ca1r"):FireServer(unpack(spawnArgs))
        wait(2.5)

        local vehicle = workspace.Vehicles:FindFirstChild(LocalPlayer.Name .. "Car")
        if not vehicle then
            warn("لم يتم العثور على الباص")
            return
        end

        -- تحريك الباص نحو اللاعب الهدف وتحريكه عشوائيًا عليه
        local targetPosition = targetPlayer.Character.HumanoidRootPart.Position
        local crazyStart = tick()
        while tick() - crazyStart < 2.5 do
            local offset = Vector3.new(
                math.random(-25, 12),
                math.random(-13, 10),
                math.random(-10, 18)
            )
            vehicle:SetPrimaryPartCFrame(CFrame.new(targetPosition + Vector3.new(0, -2, 0) + offset))
            wait(0.04)
        end

        -- إرسال الباص للمكان النهائي
        local targetDestination = Vector3.new(-86.00, -224.27, 34.57)
        vehicle:SetPrimaryPartCFrame(CFrame.new(targetDestination))
        wait(0.3)

        -- حذف الباص
        local deleteArgs = {
            [1] = "DeleteAllVehicles"
        }
        game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1Ca1r"):FireServer(unpack(deleteArgs))

        -- يرجع اللاعب لمكانه الأصلي
        humanoidRootPart.CFrame = CFrame.new(originalPosition)
    end 
})

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local SelectedPlayerName = nil

local playerNames = {}
for _, player in ipairs(Players:GetPlayers()) do
    if player ~= LocalPlayer then
        table.insert(playerNames, player.Name)
    end
end

local Dropdown = AddDropdown(Main, {
    Name = "اختر لاعب",
    Options = playerNames,
    Default = playerNames[1] or "",
    Callback = function(Value)
        SelectedPlayerName = Value
    end
})

AddButton(Main, {
    Name = "قتل بالباص عادي",
    Callback = function()
        if not SelectedPlayerName then
            warn("اختر لاعبًا أولًا")
            return
        end

        local player = LocalPlayer
        local targetPlayer = Players:FindFirstChild(SelectedPlayerName)
        if not (targetPlayer and targetPlayer.Character and targetPlayer.Character:FindFirstChild("HumanoidRootPart")) then
            warn("اللاعب غير موجود أو لم يُحمّل")
            return
        end

        local character = player.Character or player.CharacterAdded:Wait()
        local humanoid = character:WaitForChild("Humanoid")
        local humanoidRootPart = character:WaitForChild("HumanoidRootPart")

        local originalPosition = humanoidRootPart.Position
        local originalAnchoredState = humanoidRootPart.Anchored

        humanoidRootPart.CFrame = CFrame.new(1082.86, 76.00, -1125.20)
        wait(0.3)

        local spawnArgs = {
            [1] = "PickingCar",
            [2] = "SchoolBus"
        }
        game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1Ca1r"):FireServer(unpack(spawnArgs))
        wait(3.5)

        local function sitInBus()
            local vehicleName = player.Name .. "Car"
            local vehicle = workspace.Vehicles:FindFirstChild(vehicleName)
            if not vehicle then return false end

            local vehicleSeat = vehicle.Body:FindFirstChild("VehicleSeat")
            if not vehicleSeat then return false end

            humanoidRootPart.Anchored = false
            humanoid:ChangeState(Enum.HumanoidStateType.GettingUp)

            humanoidRootPart.CFrame = vehicleSeat.CFrame * CFrame.new(0, 0.3, 0)
            wait(0.15)

            humanoid.Sit = true
            firetouchinterest(humanoidRootPart, vehicleSeat, 0)
            firetouchinterest(humanoidRootPart, vehicleSeat, 1)
            wait(0.3)

            if humanoid.SeatPart ~= vehicleSeat then
                humanoidRootPart.CFrame = vehicleSeat.CFrame * CFrame.new(0, 0.3, 0)
                humanoid.Sit = true
                wait(0.3)
            end

            return humanoid.SeatPart == vehicleSeat
        end

        if not sitInBus() then return end

        local targetPosition = targetPlayer.Character.HumanoidRootPart.Position
        local vehicleName = player.Name .. "Car"
        local vehicle = workspace.Vehicles:FindFirstChild(vehicleName)

        if vehicle then
            local crazyStart = tick()
            while tick() - crazyStart < 2.5 do
                local offset = Vector3.new(
                    math.random(-25, 12),
                        math.random(-13, 10),
                        math.random(-10, 18)
                )
                vehicle:SetPrimaryPartCFrame(CFrame.new(targetPosition + Vector3.new(0, -2, 0) + offset))
                wait(0.04)
            end
        end

        local targetDestination = Vector3.new(-86.00, -224.27, 34.57)
        if vehicle then
            vehicle:SetPrimaryPartCFrame(CFrame.new(targetDestination))
            humanoidRootPart.CFrame = CFrame.new(targetDestination + Vector3.new(0, 3, 0))
        end

        wait(0.3)
        humanoidRootPart.Anchored = false
        humanoid:ChangeState(Enum.HumanoidStateType.GettingUp)

        local deleteArgs = {
            [1] = "DeleteAllVehicles"
        }
        game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1Ca1r"):FireServer(unpack(deleteArgs))

        wait(0.2)
        humanoidRootPart.CFrame = CFrame.new(originalPosition)
        humanoidRootPart.Anchored = originalAnchoredState
        humanoid:ChangeState(Enum.HumanoidStateType.RunningNoPhysics)
    end 
})

local section = AddSection(Main, {"قتل كره"})
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local ReplicatedStorage = game:GetService("ReplicatedStorage")

local player = Players.LocalPlayer
local SelectedPlayerName = nil
local flingActive = false
local flingConnection

local function clearForces(ball)
    for _, obj in ipairs(ball:GetChildren()) do
        if obj:IsA("BodyForce") or obj:IsA("BodyVelocity") or obj:IsA("BodyPosition") or obj:IsA("BodyAngularVelocity") then
            obj:Destroy()
        end
    end
end

local function applyFlingForces(ball)
    clearForces(ball)
    local force = Instance.new("BodyForce")
    force.Force = Vector3.new(1e14, 1e14, 1e14)
    force.Parent = ball

    local spin = Instance.new("BodyAngularVelocity")
    spin.AngularVelocity = Vector3.new(1e14, 1e14, 1e14)
    spin.MaxTorque = Vector3.new(math.huge, math.huge, math.huge)
    spin.P = 1e14
    spin.Parent = ball
end

local function waitForWorkspaceBall()
    local ballName = "Soccer" .. player.Name
    local ball
    repeat
        ball = workspace.WorkspaceCom["001_SoccerBalls"]:FindFirstChild(ballName)
        task.wait()
    until ball
    return ball
end

local function startFling(targetName)
    if flingActive then return end
    flingActive = true

    local targetPlayer = Players:FindFirstChild(targetName)
    if not targetPlayer or not targetPlayer.Character then
        flingActive = false
        return
    end

    local clearRemote = ReplicatedStorage.RE:FindFirstChild("1Clea1rTool1s")
    local toolRemote = ReplicatedStorage.RE:FindFirstChild("1Too1l")
    if not clearRemote or not toolRemote then
        flingActive = false
        return
    end

    clearRemote:FireServer("PlayerWantsToDeleteTool", "SoccerBall")
    toolRemote:InvokeServer("PickingTools", "SoccerBall")

    repeat task.wait() until player.Backpack:FindFirstChild("SoccerBall")
    local localBall = player.Backpack:FindFirstChild("SoccerBall")
    if not localBall then
        flingActive = false
        return
    end

    localBall.Parent = player.Character
    task.wait(0.25)

    local workspaceBall = waitForWorkspaceBall()
    applyFlingForces(workspaceBall)

    local hrp = targetPlayer.Character:FindFirstChild("HumanoidRootPart")
    local humanoid = targetPlayer.Character:FindFirstChildOfClass("Humanoid")
    if not hrp or not humanoid then
        flingActive = false
        return
    end

    local altToggle = true
    flingConnection = RunService.Heartbeat:Connect(function()
        if not flingActive then
            flingConnection:Disconnect()
            return
        end
        if not workspaceBall or not hrp or humanoid.Health <= 0 then return end

        applyFlingForces(workspaceBall)
        local speed = hrp.Velocity.Magnitude
        if speed > 1 then
            local forwardPos = hrp.Position + hrp.Velocity.Unit * 6
            workspaceBall.CFrame = CFrame.new(forwardPos + Vector3.new(0, -1, 0))
        else
            local offsetY = altToggle and 1 or -1
            workspaceBall.CFrame = CFrame.new(hrp.Position + Vector3.new(0, offsetY, 0))
            altToggle = not altToggle
        end
    end)
end

local function stopFling()
    flingActive = false
    if flingConnection then
        flingConnection:Disconnect()
        flingConnection = nil
    end
end


local playerNames = {}
for _, p in ipairs(Players:GetPlayers()) do
    if p ~= player then
        table.insert(playerNames, p.Name)
    end
end

AddDropdown(Main, {
    Name = "اختر لاعب للتطيير",
    Options = playerNames,
    Default = playerNames[1] or "",
    Callback = function(value)
        SelectedPlayerName = value
    end
})

AddButton(Main, {
    Name = "ابدأ تطيير اللاعب",
    Callback = function()
        if SelectedPlayerName then
            startFling(SelectedPlayerName)
        end
    end
})

AddButton(Main, {
    Name = "إيقاف التطيير",
    Callback = function()
        stopFling()
    end
})

local Main = MakeTab({
    Name = "اغاني مجانيه",
    Image = "rbxassetid://10734905958",
    TabTitle = false
})

local ReplicatedStorage = game:GetService("ReplicatedStorage")
local soundVolume = 100 -- مستوى الصوت الافتراضي (0-400)
local selectedSoundID = nil
local textBoxSoundID = nil
local spamThread = nil

-- 🟣 تشغيل الصوت للسيرفر (يسمعه اللاعبين)
local function Audio_All_ServerSide(ID)
    local event = ReplicatedStorage:FindFirstChild("1Gu1nSound1s", true)
    if event then
        event:FireServer(workspace, ID, math.clamp(soundVolume, 0, 400) / 100)
    else
        warn("❌ حدث الصوت غير موجود في ReplicatedStorage")
    end
end

-- 🟢 تشغيل الصوت على جهازك فقط (وهمي)
local function Audio_Client_Full(ID)
    local existingSound = workspace:FindFirstChild("FakeClientSound")
    if existingSound then
        existingSound:Stop()
        existingSound:Destroy()
    end

    local sound = Instance.new("Sound")
    sound.Name = "FakeClientSound"
    sound.SoundId = "rbxassetid://" .. ID
    sound.Volume = math.clamp(soundVolume, 0, 400) / 100
    sound.Looped = false
    sound.Parent = workspace
    sound:Play()
    sound.Ended:Connect(function()
        sound:Destroy()
    end)
end

-- 🟢 دالة لتشغيل الأغنية وهميًا + على السيرفر
local function Play_Full(ID)
    if not ID then return end
    -- 🌐 شغل للسيرفر (يسمعه اللاعبين)
    Audio_All_ServerSide(ID)
    -- 👂 شغل وهمي عندك
    Audio_Client_Full(ID)
end

-- 🟢 TextBox لإدخال ID يدوي
AddTextBox(Main, {
    Name = "ضع كود الاغنيه",
    PlaceholderText = "هنا",
    ClearText = true,
    Callback = function(Value)
        local id = tonumber(Value)
        if id then
            textBoxSoundID = id
            print("✅ تم إدخال ID يدوي: " .. id)
        else
            warn("❌ أدخل رقم ID صحيح فقط")
            textBoxSoundID = nil
        end
    end
})

-- 🟢 Dropdown لأغاني جاهزة
AddDropdown(Main, {
    Name = "اغاني منوعه",
    Default = "Xingamento",
    Options = {
        "Xingamento", "Baldi Basic's Glitch", "Sucumba", "Seek Jumpscare", "DogDay Jumpscare",
        "Springtrap Jumpscare", "Foxy Jumpscare", "Laugh", "Skull's Laugh", "Laugh Boss",
        "C00lkid No Fear!", "C00lkid Hahaha", "SirenHead", "Tubers93", "Audio Glitcher Sound",
        "Oof Sound", "Buuuh Sound", "My Heart Is Pure Evil Sound", "Laugh Sound"
    },
    Callback = function(selected)
        selectedSoundID = ({
            ["Xingamento"] = 8232773326,
            ["Baldi Basic's Glitch"] = 98207961689599,
            ["Sucumba"] = 7946300950,
            ["Seek Jumpscare"] = 133358860191747,
            ["DogDay Jumpscare"] = 132162728926958,
            ["Springtrap Jumpscare"] = 17609408193,
            ["Foxy Jumpscare"] = 6949978667,
            ["Laugh"] = 140395748019933,
            ["Skull's Laugh"] = 100609956908791,
            ["Laugh Boss"] = 6963880809,
            ["C00lkid No Fear!"] = 126083075694948,
            ["C00lkid Hahaha"] = 102348131944238,
            ["SirenHead"] = 5681392074,
            ["Tubers93"] = 103215672097028,
            ["Audio Glitcher Sound"] = 7236490488,
            ["Oof Sound"] = 6598984092,
            ["Buuuh Sound"] = 83788010495185,
            ["My Heart Is Pure Evil Sound"] = 106843479364998,
            ["Laugh Sound"] = 123106903091799,
        })[selected]
        print("✅ تم اختيار أغنية جاهزة: " .. selected .. " | ID: " .. tostring(selectedSoundID))
    end
})

-- 🟢 TextBox لمستوى الصوت
AddTextBox(Main, {
    Name = " ضع مستوى الصوت (0 - 400)",
    PlaceholderText = "حدد الرقم",
    ClearText = true,
    Callback = function(Value)
        local vol = tonumber(Value)
        if vol and vol >= 0 and vol <= 400 then
            soundVolume = vol
            print("🔊 تم ضبط مستوى الصوت إلى: " .. vol)
        else
            warn("❌ أدخل رقم بين 0 و 400 فقط")
        end
    end
})

-- 🟢 دالة لجلب ID الحالي
local function getCurrentSoundID()
    return textBoxSoundID or selectedSoundID
end

-- 🟢 زر سبام وهمي للأغنية
AddToggle(Main, {
    Name = "سبام اغنيه",
    Default = false,
    Callback = function(Value)
        if Value then
            print("✅ بدأ سبام الأغنية الوهمي")
            spamThread = task.spawn(function()
                while Value do
                    local id = getCurrentSoundID()
                    if id then
                        Play_Full(id)
                        task.wait(2) -- سبام كل 2 ثانية
                    else
                        warn("❌ لم يتم تحديد ID")
                        break
                    end
                end
            end)
        else
            print("⛔ تم إيقاف السبام الوهمي")
            if spamThread then
                task.cancel(spamThread)
                spamThread = nil
            end
        end
    end
})

-- 🟢 زر تشغيل لمرة واحدة
AddButton(Main, {
    Name = "تشغيل الاغنيه",
    Callback = function()
        local id = getCurrentSoundID()
        if id then
            Play_Full(id)
            print("▶️ تشغيل وهمي للأغنية ID: " .. tostring(id))
        else
            warn("❌ لم يتم تحديد ID")
        end
    end
})







local section = AddSection(Main, {"اغاني montagem فخمه"})

local ReplicatedStorage = game:GetService("ReplicatedStorage")
local soundVolume = 100 -- مستوى الصوت الافتراضي (0-400)
local selectedSoundID = nil
local textBoxSoundID = nil
local spamThread = nil

-- 🟣 تشغيل الصوت للسيرفر (يسمعه اللاعبين)
local function Audio_All_ServerSide(ID)
    local event = ReplicatedStorage:FindFirstChild("1Gu1nSound1s", true)
    if event then
        event:FireServer(workspace, ID, math.clamp(soundVolume, 0, 400) / 100)
    else
        warn("❌ حدث الصوت غير موجود في ReplicatedStorage")
    end
end

-- 🟢 تشغيل الصوت على جهازك فقط (وهمي)
local function Audio_Client_Full(ID)
    local existingSound = workspace:FindFirstChild("FakeClientSound")
    if existingSound then
        existingSound:Stop()
        existingSound:Destroy()
    end

    local sound = Instance.new("Sound")
    sound.Name = "FakeClientSound"
    sound.SoundId = "rbxassetid://" .. ID
    sound.Volume = math.clamp(soundVolume, 0, 400) / 100
    sound.Looped = false
    sound.Parent = workspace
    sound:Play()
    sound.Ended:Connect(function()
        sound:Destroy()
    end)
end

-- 🟢 دالة لتشغيل الأغنية وهميًا + على السيرفر
local function Play_Full(ID)
    if not ID then return end
    -- 🌐 شغل للسيرفر (يسمعه اللاعبين)
    Audio_All_ServerSide(ID)
    -- 👂 شغل وهمي عندك
    Audio_Client_Full(ID)
end



-- 🟢 Dropdown لأغاني Montagem
AddDropdown(Main, {
    Name = "Montagens",
    Default = "Montagem 1",
    Options = {
        "Montagem 1",
        "Montagem 2",
        "Montagem 3",
        "Montagem 4",
        "Montagem 5",
        "Montagem 6",
        "Montagem 7"
    },
    Callback = function(selected)
        selectedSoundID = ({
            ["Montagem 1"] = 95231133674951,
            ["Montagem 2"] = 103522488156551,
            ["Montagem 3"] = 101974874883467,
            ["Montagem 4"] = 107424491541808,
            ["Montagem 5"] = 122697381252723,
            ["Montagem 6"] = 120138115344262,
            ["Montagem 7"] = 132358528258642,
        })[selected]
        print("✅ تم اختيار مونتاج: " .. selected .. " | ID: " .. tostring(selectedSoundID))
    end
})


-- 🟢 دالة لجلب ID الحالي
local function getCurrentSoundID()
    return textBoxSoundID or selectedSoundID
end



-- 🟢 زر تشغيل لمرة واحدة
AddButton(Main, {
    Name = "تشغيل الاغنيه",
    Callback = function()
        local id = getCurrentSoundID()
        if id then
            Play_Full(id)
            print("▶️ تشغيل وهمي للأغنية ID: " .. tostring(id))
        else
            warn("❌ لم يتم تحديد ID")
        end
    end
})

local Main = MakeTab({
    Name = "اغاني جيم باس",
    Image = "rbxassetid://10734905823",
    TabTitle = false
})

local Paragraph = AddParagraph(Main, {"اضغط على زر الريست لتفعيل الاغاني"})

AddButton(Main, {
Name = "الريست",
Callback = function()
  game.Players.LocalPlayer.Character.Humanoid.Health = 0
end
})
local section = AddSection(Main, {"مشغل الاغاني"})

--[[
  Name = "Main" <string> Nome da guia
]]

local Slider = AddSlider(Main, {
    Name = "مستوى صوت الاغاني",
    MinValue = 0,
    MaxValue = 10,
    Default = 5,
    Increase = 0.1,
    Callback = function(Value)
        local sound = workspace:FindFirstChild("Song") or game.Players.LocalPlayer.PlayerGui:FindFirstChild("Song")
        if sound and sound:IsA("Sound") then
            sound.Volume = Value
        else
            warn("لم يتم العثور على صوت لتغيير مستوى الصوت")
        end
    end
})


AddTextBox(Main, {
    Name = "ادخل كود الأغنية للبيت",
    Default = "",
    PlaceholderText = "يرجى كتابه الكود صح ليصل الى البيت",
    ClearText = true,
    Callback = function(Value)
        -- التأكد أن اللاعب يملك بيت
        local house = game:GetService("Workspace").Houses:FindFirstChild(Players.LocalPlayer.Name .. "House")
        if house then
            -- إرسال الكود لتشغيل الأغنية على البيت
            local args = {
                [1] = "PickingHouseMusic",
                [2] = Value -- هنا نستخدم الكود المدخل لتشغيل الأغنية
            }
            game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1Ho1u1s1e"):FireServer(unpack(args))
            print("✅ تم تشغيل الأغنية على البيت ID: " .. Value)
        else
            warn("❌ لم يتم العثور على بيتك. ضع بيت أولاً!")
        end
    end
})


AddTextBox(Main, {
    Name = " ادخل كود الأغنية للسيارة",
    Default = "",
    PlaceholderText = "يرجى كتابه id صالح ليصل الى سيارتك",
    ClearText = true,
    Callback = function(Value)
        -- التأكد أن اللاعب يملك سيارة
        local car = workspace.Vehicles:FindFirstChild(Players.LocalPlayer.Name .. "Car")
        if car then
            -- إرسال الكود لتشغيل الأغنية على السيارة
            local args = {
                [1] = "PickingCarMusicText",
                [2] = Value -- هنا نستخدم الكود المدخل لتشغيل الأغنية
            }
            game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1Ca1r"):FireServer(unpack(args))
            print("✅ تم تشغيل الأغنية على السيارة ID: " .. Value)
        else
            warn("❌ لم يتم العثور على سيارتك. استدعي سيارة أولاً!")
        end
    end
})

AddTextBox(Main, {
    Name = "ادخل كود الأغنية للسكيت بورد",
    Default = "",
    PlaceholderText = "اكتب ID الأغنية هنا",
    ClearText = true,
    Callback = function(Value)
        -- تجهيز السكيت بورد
        local args1 = {
            [1] = "SkateBoard"
        }
        game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1NoMoto1rVehicle1s"):FireServer(unpack(args1))
        wait(1) -- مهلة صغيرة لتجهيز السكيت بورد

        -- تشغيل الأغنية على السكيت بورد
        local args2 = {
            [1] = "ScooterCustomMusic",
            [2] = Value -- الكود المدخل من اللاعب
        }
        game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1NoMoto1rVehicle1s"):FireServer(unpack(args2))
        
        print("✅ تم تشغيل الأغنية على السكيت بورد ID: " .. Value)
    end
})

local ReplicatedStorage = game:GetService("ReplicatedStorage")
local soundVolume = 100 -- مستوى الصوت الافتراضي (0-400)
local selectedSoundID = nil
local textBoxSoundID = nil
local spamThread = nil

-- 🟣 تشغيل الصوت للسيرفر (يسمعه اللاعبين)
local function Audio_All_ServerSide(ID)
    local event = ReplicatedStorage:FindFirstChild("1Gu1nSound1s", true)
    if event then
        event:FireServer(workspace, ID, math.clamp(soundVolume, 0, 400) / 100)
    else
        warn("❌ حدث الصوت غير موجود في ReplicatedStorage")
    end
end

-- 🟢 تشغيل الصوت على جهازك فقط (وهمي)
local function Audio_Client_Full(ID)
    local existingSound = workspace:FindFirstChild("FakeClientSound")
    if existingSound then
        existingSound:Stop()
        existingSound:Destroy()
    end

    local sound = Instance.new("Sound")
    sound.Name = "FakeClientSound"
    sound.SoundId = "rbxassetid://" .. ID
    sound.Volume = math.clamp(soundVolume, 0, 400) / 100
    sound.Looped = false
    sound.Parent = workspace
    sound:Play()
    sound.Ended:Connect(function()
        sound:Destroy()
    end)
end

-- 🟢 دالة لتشغيل الأغنية وهميًا + على السيرفر
local function Play_Full(ID)
    if not ID then return end
    -- 🌐 شغل للسيرفر (يسمعه اللاعبين)
    Audio_All_ServerSide(ID)
    -- 👂 شغل وهمي عندك
    Audio_Client_Full(ID)
end

-- 🟢 TextBox لإدخال ID يدوي
AddTextBox(Main, {
    Name = "ضع كود الاغنيه",
    PlaceholderText = "هنا",
    ClearText = true,
    Callback = function(Value)
        local id = tonumber(Value)
        if id then
            textBoxSoundID = id
            print("✅ تم إدخال ID يدوي: " .. id)
        else
            warn("❌ أدخل رقم ID صحيح فقط")
            textBoxSoundID = nil
        end
    end
})


AddButton(Main, {
  Name = "فونك 1",
  Callback = function()
    	local args = {
    [1] = "SkateBoard"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
local args = {
    [1] = "PickingScooterMusicText",
    [2] = "17647322226"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
  end
})



AddButton(Main, {
  Name = "فونك2",
  Callback = function()
    	local args = {
    [1] = "SkateBoard"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
local args = {
    [1] = "PickingScooterMusicText",
    [2] = "115859025716354"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
  end
})



AddButton(Main, {
  Name = "فونك3",
  Callback = function()
    	local args = {
    [1] = "SkateBoard"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
local args = {
    [1] = "PickingScooterMusicText",
    [2] = "73966367524216"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
  end
})


AddButton(Main, {
  Name = "فونك4",
  Callback = function()
    	local args = {
    [1] = "SkateBoard"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
local args = {
    [1] = "PickingScooterMusicText",
    [2] = "76578817848504"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
  end
})


AddButton(Main, {
  Name = "فونك5",
  Callback = function()
    	local args = {
    [1] = "SkateBoard"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
local args = {
    [1] = "PickingScooterMusicText",
    [2] = "93218265275853"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
  end
})



AddButton(Main, {
  Name = "فونك6",
  Callback = function()
    	local args = {
    [1] = "SkateBoard"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
local args = {
    [1] = "PickingScooterMusicText",
    [2] = "102402883551679"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
  end
})

AddButton(Main, {
  Name = "فونك7",
  Callback = function()
    	local args = {
    [1] = "SkateBoard"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
local args = {
    [1] = "PickingScooterMusicText",
    [2] = "103066073385622"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
  end
})


AddButton(Main, {
  Name = "فونك8",
  Callback = function()
    	local args = {
    [1] = "SkateBoard"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
local args = {
    [1] = "PickingScooterMusicText",
    [2] = "75038862482887"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
  end
})


AddButton(Main, {
  Name = "فونك9",
  Callback = function()
    	local args = {
    [1] = "SkateBoard"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
local args = {
    [1] = "PickingScooterMusicText",
    [2] = "91301048841024"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
  end
})


AddButton(Main, {
  Name = "فونك10",
  Callback = function()
    	local args = {
    [1] = "SkateBoard"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
local args = {
    [1] = "PickingScooterMusicText",
    [2] = "96527800313172"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
  end
})


AddButton(Main, {
  Name = "فونك11",
  Callback = function()
    	local args = {
    [1] = "SkateBoard"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
local args = {
    [1] = "PickingScooterMusicText",
    [2] = "115028690484951"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
  end
})



AddButton(Main, {
  Name = "فونك 12",
  Callback = function()
    	local args = {
    [1] = "SkateBoard"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
local args = {
    [1] = "PickingScooterMusicText",
    [2] = "91161894069716"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
  end
})
AddButton(Main, {
  Name = " اغنية فونك رقم 13",
  Callback = function()
     local args = {
    [1] = "SkateBoard"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
local args = {
    [1] = "PickingScooterMusicText",
    [2] = "16662833837"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
  end
})
AddButton(Main, {
  Name = " اغنية فونك رقم 14",
  Callback = function()
     local args = {
    [1] = "SkateBoard"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
local args = {
    [1] = "PickingScooterMusicText",
    [2] = "90698302380427"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
  end
})
AddButton(Main, {
  Name = " اغنية فونك رقم 15",
  Callback = function()
     local args = {
    [1] = "SkateBoard"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
local args = {
    [1] = "PickingScooterMusicText",
    [2] = "16831108393"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
  end
})
AddButton(Main, {
  Name = " اغنية فونك رقم 16",
  Callback = function()
     local args = {
    [1] = "SkateBoard"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
local args = {
    [1] = "PickingScooterMusicText",
    [2] = "78099799004483"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
  end
})
AddButton(Main, {
  Name = "اغنية فونك رقم 17",
  Callback = function()
     local args = {
    [1] = "SkateBoard"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
local args = {
    [1] = "PickingScooterMusicText",
    [2] = "138106899521204"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
  end
})
AddButton(Main, {
  Name = "اغنية فونك رقم 18",
  Callback = function()
     local args = {
    [1] = "SkateBoard"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
local args = {
    [1] = "PickingScooterMusicText",
    [2] = "84223825719510"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
  end
})
AddButton(Main, {
  Name = "اغنية فونك رقم 19",
  Callback = function()
     local args = {
    [1] = "SkateBoard"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
local args = {
    [1] = "PickingScooterMusicText",
    [2] = "79314929106323"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
  end
})
AddButton(Main, {
  Name = " اغنيه فونك رقم 20",
  Callback = function()
     local args = {
    [1] = "SkateBoard"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
local args = {
    [1] = "PickingScooterMusicText",
    [2] = "113312785512702"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
  end
})


AddSection(Main, {"اغاني مشهوره وحلوه"})

AddButton(Main, {
  Name = "اغنيه مشهوره 1",
  Callback = function()
    	local args = {
    [1] = "SkateBoard"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
local args = {
    [1] = "PickingScooterMusicText",
    [2] = "76463442516219"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
  end
})


AddButton(Main, {
  Name = "اغنيه مشهوره 2",
  Callback = function()
    	local args = {
    [1] = "SkateBoard"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
local args = {
    [1] = "PickingScooterMusicText",
    [2] = "1846687233"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
  end
})


AddButton(Main, {
  Name = "اغـنيـة مـشـهورة 3",
  Callback = function()
    	local args = {
    [1] = "SkateBoard"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
local args = {
    [1] = "PickingScooterMusicText",
    [2] = "72515474996038"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
  end
})
AddSection(Main, {"عـراقـي"})

AddButton(Main, {
  Name = "يا حتـه من كـلبي",
  Callback = function()
    	local args = {
    [1] = "SkateBoard"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
local args = {
    [1] = "PickingScooterMusicText",
    [2] = "119630965566559"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
  end
})

AddButton(Main, {
  Name = "اغنية عراقيه",
  Callback = function()
     local args = {
    [1] = "SkateBoard"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
local args = {
    [1] = "PickingScooterMusicText",
    [2] = "11463392143"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
  end
})
AddButton(Main, {
  Name = "القول قول الصوارم",
  Callback = function()
     local args = {
    [1] = "SkateBoard"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
local args = {
    [1] = "PickingScooterMusicText",
    [2] = "135009652401688"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
  end
})
AddButton(Main, {
  Name = "عـفـت كـلبـي",
  Callback = function()
    	local args = {
    [1] = "SkateBoard"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
local args = {
    [1] = "PickingScooterMusicText",
    [2] = "131004009162099"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
  end
})


AddButton(Main, {
  Name = "عراقي",
  Callback = function()
    	local args = {
    [1] = "SkateBoard"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
local args = {
    [1] = "PickingScooterMusicText",
    [2] = "80039364766636"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
  end
})


AddButton(Main, {
  Name = "العراقي لو نزل",
  Callback = function()
    	local args = {
    [1] = "SkateBoard"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
local args = {
    [1] = "PickingScooterMusicText",
    [2] = "111256095783364"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
  end
})


AddButton(Main, {
  Name = "كل عقلگ تحجي",
  Callback = function()
    	local args = {
    [1] = "SkateBoard"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
local args = {
    [1] = "PickingScooterMusicText",
    [2] = "122774951440401"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
  end
})


AddButton(Main, {
  Name = "ما اريدك تهويني",
  Callback = function()
    	local args = {
    [1] = "SkateBoard"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
local args = {
    [1] = "PickingScooterMusicText",
    [2] = "135911328646170"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
  end
})


AddButton(Main, {
  Name = "جاي انساك",
  Callback = function()
    	local args = {
    [1] = "SkateBoard"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
local args = {
    [1] = "PickingScooterMusicText",
    [2] = "72918998227337"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
  end
})


AddButton(Main, {
  Name = "نجوم الدنيا",
  Callback = function()
    	local args = {
    [1] = "SkateBoard"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
local args = {
    [1] = "PickingScooterMusicText",
    [2] = "73632319736202"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
  end
})


AddSection(Main, {"اغاني عربيه"})


AddButton(Main, {
  Name = "ليه ساكت",
  Callback = function()
    	local args = {
    [1] = "SkateBoard"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
local args = {
    [1] = "PickingScooterMusicText",
    [2] = "119437864395329"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
  end
})


AddButton(Main, {
  Name = "ااه ياحلوو",
  Callback = function()
    	local args = {
    [1] = "SkateBoard"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
local args = {
    [1] = "PickingScooterMusicText",
    [2] = "93620598835551"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
  end
})
AddButton(Main, {
  Name = "تعالي بيبي عند دادي",
  Callback = function()
     local args = {
    [1] = "SkateBoard"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
local args = {
    [1] = "PickingScooterMusicText",
    [2] = "7984027399"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
  end
})
AddButton(Main, {
  Name = "يا بكايه",
  Callback = function()
    	local args = {
    [1] = "SkateBoard"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
local args = {
    [1] = "PickingScooterMusicText",
    [2] = "98640789490482"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
  end
})


AddButton(Main, {
  Name = "صدقني اكرهك",
  Callback = function()
    	local args = {
    [1] = "SkateBoard"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
local args = {
    [1] = "PickingScooterMusicText",
    [2] = "113478978326245"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
  end
})


AddButton(Main, {
  Name = "انا دمي فلسطين",
  Callback = function()
    	local args = {
    [1] = "SkateBoard"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
local args = {
    [1] = "PickingScooterMusicText",
    [2] = "84112690044490"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
  end
})


AddButton(Main, {
  Name = "كردي",
  Callback = function()
    	local args = {
    [1] = "SkateBoard"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
local args = {
    [1] = "PickingScooterMusicText",
    [2] = "134738554464984"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
  end
})


AddButton(Main, {
  Name = "هجوله",
  Callback = function()
    	local args = {
    [1] = "SkateBoard"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
local args = {
    [1] = "PickingScooterMusicText",
    [2] = "99472699182002"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
  end
})


AddButton(Main, {
  Name = "تحرير سوريا",
  Callback = function()
    	local args = {
    [1] = "SkateBoard"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
local args = {
    [1] = "PickingScooterMusicText",
    [2] = "87510423075068"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
  end
})
AddSection(Main, {"دبكات"})
AddButton(Main, {
  Name = "دبكه 1",
  Callback = function()
    	local args = {
    [1] = "SkateBoard"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
local args = {
    [1] = "PickingScooterMusicText",
    [2] = "76698985299412"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
  end
})


AddButton(Main, {
  Name = "دبكه 2",
  Callback = function()
    	local args = {
    [1] = "SkateBoard"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
local args = {
    [1] = "PickingScooterMusicText",
    [2] = "90807238125839"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
  end
})

AddSection(Main, {"سب"})
AddButton(Main, {
  Name = "سب 1",
  Callback = function()
     local args = {
    [1] = "SkateBoard"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
local args = {
    [1] = "PickingScooterMusicText",
    [2] = "6536444735"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
  end
})
AddButton(Main, {
    Name = "سب 2",
    Callback = function()
  
  local args = {
      [1] = "PickingScooterMusicText",
      [2] = "6536444735"
  }
  
  game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
  
    end
  })
  
  AddButton(Main, {
    Name = "3سب",
    Callback = function()
  
  local args = {
      [1] = "PickingScooterMusicText",
      [2] = "8701632845"
  }
  
  game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
  
    end
  })
  
AddButton(Main, {
  Name = "سب 4",
  Callback = function()
     local args = {
    [1] = "SkateBoard"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
local args = {
    [1] = "PickingScooterMusicText",
    [2] = "8701632845"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
  end
})
AddButton(Main, {
  Name = "سب 5",
  Callback = function()
     local args = {
    [1] = "SkateBoard"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
local args = {
    [1] = "PickingScooterMusicText",
    [2] = "6713993281"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
  end
})
AddButton(Main, {
  Name = "سب 6",
  Callback = function()
     local args = {
    [1] = "SkateBoard"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
local args = {
    [1] = "PickingScooterMusicText",
    [2] = "5849978429"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
  end
})
AddButton(Main, {
  Name = "سب 7",
  Callback = function()
     local args = {
    [1] = "SkateBoard"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
local args = {
    [1] = "PickingScooterMusicText",
    [2] = "7183326833"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
  end
})
AddSection(Main, {"قران"})
AddButton(Main, {
    Name = "قران 1",
    Callback = function()
  
  local args = {
      [1] = "PickingScooterMusicText",
      [2] = "7609175072"
  }
  
  game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
  
    end
  })
  
  AddButton(Main, {
    Name = "قران 2",
    Callback = function()
  
  local args = {
      [1] = "PickingScooterMusicText",
      [2] = "9108676586"
  }
  
  game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
  
    end
  })

AddButton(Main, {
  Name = "قران 3",
  Callback = function()
    	local args = {
    [1] = "SkateBoard"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
local args = {
    [1] = "PickingScooterMusicText",
    [2] = "89807249526206"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
  end
})


AddButton(Main, {
  Name = "قران 4",
  Callback = function()
    	local args = {
    [1] = "SkateBoard"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
local args = {
    [1] = "PickingScooterMusicText",
    [2] = "1836685929"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
  end
})


AddButton(Main, {
  Name = "قران5",
  Callback = function()
    	local args = {
    [1] = "SkateBoard"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
local args = {
    [1] = "PickingScooterMusicText",
    [2] = "113184639716766"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
  end
})


AddButton(Main, {
  Name = "قران6",
  Callback = function()
    	local args = {
    [1] = "SkateBoard"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
local args = {
    [1] = "PickingScooterMusicText",
    [2] = "113267251759253"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
  end
})


AddButton(Main, {
  Name = "قـرأن7",
  Callback = function()
    	local args = {
    [1] = "SkateBoard"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
local args = {
    [1] = "PickingScooterMusicText",
    [2] = "104251523505775"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
  end
})


AddButton(Main, {
  Name = "قـرأن8",
  Callback = function()
    	local args = {
    [1] = "SkateBoard"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
local args = {
    [1] = "PickingScooterMusicText",
    [2] = "94610377976036"
}
 
game:GetService("ReplicatedStorage").RE:FindFirstChild("1NoMoto1rVehicle1s"):FireServer(unpack(args))
  end
})

local Main = MakeTab({
    Name = "اسماء",
    Image = "rbxassetid://10747373426",
    TabTitle = false
})


-- 🔄 تتبع حالة التلوين
local isNameActive = false
local isBioActive = false

-- 🌈 زر لتلوين الاسم
AddToggle(Main, {
    Name = "تـلـويـن الاسـم ",
    Default = false,
    Callback = function(value)
        isNameActive = value
        print(value and "✅ تفعيل تلوين الاسم" or "❌ إيقاف تلوين الاسم")
    end
})

-- 🌈 زر لتلوين البايو
AddToggle(Main, {
    Name = "تـلـويـن الـبـايـو ",
    Default = false,
    Callback = function(value)
        isBioActive = value
        print(value and "✅ تفعيل تلوين البايو" or "❌ إيقاف تلوين البايو")
    end
})

-- 🎨 ألوان زاهية للتأثير
local vibrantColors = {
    Color3.fromRGB(255, 0, 0),
    Color3.fromRGB(0, 255, 0),
    Color3.fromRGB(0, 0, 255),
    Color3.fromRGB(255, 255, 0),
    Color3.fromRGB(255, 0, 255),
    Color3.fromRGB(0, 255, 255),
    Color3.fromRGB(255, 165, 0),
    Color3.fromRGB(128, 0, 128),
    Color3.fromRGB(255, 20, 147)
}


-- 🔁 تلوين الاسم (مستمر)
task.spawn(function()
    while true do
        if isNameActive then
            local color = vibrantColors[math.random(#vibrantColors)]
            local args = {
                [1] = "PickingRPNameColor",
                [2] = color
            }
            game:GetService("ReplicatedStorage").RE:FindFirstChild("1RPNam1eColo1r"):FireServer(unpack(args))
        end
        task.wait(0.7)
    end
end)

-- 🔁 تلوين البايو (مستمر)
task.spawn(function()
    while true do
        if isBioActive then
            local color = vibrantColors[math.random(#vibrantColors)]
            local args = {
                [1] = "PickingRPBioColor",
                [2] = color
            }
            game:GetService("ReplicatedStorage").RE:FindFirstChild("1RPNam1eColo1r"):FireServer(unpack(args))
        end
        task.wait(0.7)
    end
end)
AddSection(Main, {"سبام اسماء"})

local spamming = false
local names = {
    "عمك غالي",
    "خالد",
    "ماجد",
    "ريان",
    "جاسم",
    "عبدالله",
    "فهد",
    "تركي",
    "ناصر",
    "يزن"
}

AddToggle(Main, {
    Name = " سبام أسماء",
    Default = false,
    Callback = function(value)
        spamming = value
        if spamming then
            task.spawn(function()
                while spamming do
                    local randomName = names[math.random(1, #names)]
                    local args = {
                        [1] = "RolePlayName",
                        [2] = randomName
                    }
                    game:GetService("ReplicatedStorage").RE:FindFirstChild("1RPNam1eTex1t"):FireServer(unpack(args))
                    task.wait(0.25) -- تقدر تخففها أو تزيدها حسب سرعة السبام
                end
            end)
        end
    end
})

AddSection(Main, {"الاسـم"})

AddTextBox(Main, {
Name = "ضـع اسـم",
Default = "",
PlaceholderText = "الاسم",
ClearText = true,
Callback = function(value)
local args = {[1] = "RolePlayName", [2] = value};
        game:GetService("ReplicatedStorage").RE:FindFirstChild(
            "1RPNam1eTex1t"):FireServer(unpack(args));
    end
})

AddSection(Main, {"الـبـايـو"})

AddTextBox(Main, {
Name = "ضـع الـبـايـو",
Default = "",
PlaceholderText = "ضع بايو",
ClearText = true,
Callback = function(value)
     local args = {[1] = "RolePlayBio", [2] = value};
        game:GetService("ReplicatedStorage").RE:FindFirstChild(
            "1RPNam1eTex1t"):FireServer(unpack(args));
    end
})


-- بايوات جاهزة
local selectedBio = ""
AddDropdown(Main, {
    Name = "اختـر بـايـو",
    Default = "",
    Options = {
        "<_HooR_>",
        "<Œ‘A-h-M-a-Dً>",
        "فرح",
        "H-amuddi^-^‘",
        ":)sadq(:",
        "AFK",
        "<عبوصـيـٍُٰ>",
        "⚜️<A_l_i>⚜️",
        "[🏖️Chill Mode🏖️]",
        "💀 DoN't MeSs WiTh Me 💀",
        "💔 Broken 💔",
        "🕶️ Cool but Deadly",
        "🧸 Cutie in Chaos",
        "♠️ The Real One ♠️",
        "😈 Silent Hunter 😈",
        "🥷 N1nja_M0de",
        "✨ Dreamer ✨",
        "☕ AFK but watching",
        "🎧 Music is life 🎧"
    },
    Callback = function(value)
        selectedBio = value
    end
})
AddButton(Main, {
    Name = "تطبيق البايو",
    Callback = function()
        if selectedBio ~= "" then
            local args = {
                [1] = "RolePlayBio",
                [2] = selectedBio
            }
            game:GetService("ReplicatedStorage").RE:FindFirstChild("1RPNam1eTex1t"):FireServer(unpack(args))
        end
    end
})


-- أسماء أولاد
local selectedBoyName = ""
AddDropdown(Main, {
    Name = "اختر اسم ولد",
    Default = "",
    Options = {
        "🧣رشــــــــــــــــِــودي🧣",
        "عبدالله ",
        "خالد",
        "مــحــــمـــًًد",
        "راكان الـوحـش",
        "A7mad⚡",
        "يـزيـد الـسـنـد🛡️",
        "Faisal🔥",
        "الذيب🐺",
        "No0r🕶️",
        "نـواف الـمـحنك🎯",
        "S3ood💪",
        "T̷A̷L̷A̷L̷🔥",
        "Majed⚔️",
        "بدر👑",
        "SULTAN💥",
        "Zeyad🚀",
        "نـاصـر الأسطـوري",
        "Abood😎",
        "فهد الناري🔥"
    },
    Callback = function(value)
        selectedBoyName = value
    end
})
AddButton(Main, {
    Name = "تطبيق اسم الولد",
    Callback = function()
        if selectedBoyName ~= "" then
            local args = {
                [1] = "RolePlayName",
                [2] = selectedBoyName
            }
            game:GetService("ReplicatedStorage").RE:FindFirstChild("1RPNam1eTex1t"):FireServer(unpack(args))
        end
    end
})


-- أسماء بنات
local selectedGirlName = ""
AddDropdown(Main, {
    Name = "اختر اسم بنت",
    Default = "",
    Options = {
        "غُـًــــــــــلَأّ",
        "فُــــــــــــرحً",
        "غزل",
        "جمانه",
        "نارين",
        "نِسِـريِّنِ",
        "𝓛𝓪𝓶𝓪✨",
        "نُـوف الـهـادئـة🌸",
        "مـيـمـا الـكـيـوت💗",
        "𝒜𝒷𝓇𝒾𝓁𝓁𝒶🌼",
        "تُـوتـه🧁",
        "Ro0o7i💋",
        "لُجين💎",
        "𝒮𝓊𝒹𝒶𝓃𝒾𝒶💃",
        "ديـم🦋",
        "جـود الـفخمـة👑",
        "Huda🌙",
        "𝒴𝒶𝓈𝓂𝒾𝓃𝑒🌷",
        "ريـم الـجمـيلة🦢",
        "𝓑𝓪𝓫𝔂 𝓖𝓲𝓻𝓵💞"
    },
    Callback = function(value)
        selectedGirlName = value
    end
})
AddButton(Main, {
    Name = "تطبيق اسم البنت",
    Callback = function()
        if selectedGirlName ~= "" then
            local args = {
                [1] = "RolePlayName",
                [2] = selectedGirlName
            }
            game:GetService("ReplicatedStorage").RE:FindFirstChild("1RPNam1eTex1t"):FireServer(unpack(args))
        end
    end
})

local Main = MakeTab({
    Name = "التنقل",
    Image = "rbxassetid://10734886202",
    TabTitle = false
})
local section = AddSection(Main, {"تروح للاماكن"})
local selectedLocation = nil

AddDropdown(Main, {
    Name = "اختـر المـكان",
    Default = "اختر",
    Options = {
        "البيت 1", "البيت 2", "البيت 3", "البيت 4", "البيت 5", "البيت 6", "البيت 7",
        "البيت 11", "البيت 12", "البيت 13", "البيت 14", "البيت 15", "البيت 16", "البيت 17",
        "البيت 18", "البيت 20", "البيت 21", "البيت 22", "البيت 23", "البيت 24",
        "البيت 28", "البيت 29", "البيت 30", "البيت 31", "البيت 32", "البيت 33", "البيت 34", "البيت 35", "البيت 36",
        "خلف البيوت", "امام البيوت", "مكان اسلحة", "بيت مسكون", "مكان سري", "قهوة", "البداية",
        "مستشفى", "فوق الجسر", "مكان الكهرباء", "جوه الأرض", "جوه الأرض 2", "جوه الأرض 3",
        "جزيرة 1", "مطار", "البيوت", "نص الشارع", "فوق الجبل", "فوق المدرسة"
    },
    Callback = function(value)
        selectedLocation = value
    end
})

local locations = {
    ["البيت 1"] = CFrame.new(258, 5, 222),
    ["البيت 2"] = CFrame.new(221, 5, 230),
    ["البيت 3"] = CFrame.new(262, 20, 223),
    ["البيت 4"] = CFrame.new(222, 20, 226),
    ["البيت 5"] = CFrame.new(159, 20, 226),
    ["البيت 6"] = CFrame.new(-34, 17, -119),
    ["البيت 7"] = CFrame.new(-38, 33, -119),
    ["البيت 11"] = CFrame.new(-21, 32, 436),
    ["البيت 12"] = CFrame.new(155, 32, 433),
    ["البيت 13"] = CFrame.new(255, 33, 431),
    ["البيت 14"] = CFrame.new(245, 32, 394),
    ["البيت 15"] = CFrame.new(145, 32, 391),
    ["البيت 16"] = CFrame.new(-24, 32, 390),
    ["البيت 17"] = CFrame.new(-185, 32, -248),
    ["البيت 18"] = CFrame.new(-346, 32, -248),
    ["البيت 20"] = CFrame.new(-460, 32, -291),
    ["البيت 21"] = CFrame.new(-353, 32, -291),
    ["البيت 22"] = CFrame.new(-193, 32, -291),
    ["البيت 23"] = CFrame.new(-409, 64, -441),
    ["البيت 24"] = CFrame.new(-359, 63, -495),
    ["البيت 28"] = CFrame.new(-98, 1, 1075),
    ["البيت 29"] = CFrame.new(-733, 1, 778),
    ["البيت 30"] = CFrame.new(-232, -4, 788),
    ["البيت 31"] = CFrame.new(611, 72, -333),
    ["البيت 32"] = CFrame.new(-878, 1, -344),
    ["البيت 33"] = CFrame.new(-144, 64, -417),
    ["البيت 34"] = CFrame.new(261, 32, 566),
    ["البيت 35"] = CFrame.new(-31, 0, 2210),
    ["البيت 36"] = CFrame.new(249, 21, -2295),
    ["خلف البيوت"] = CFrame.new(192, 4, 272),
    ["امام البيوت"] = CFrame.new(136, 4, 117),
    ["مكان اسلحة"] = CFrame.new(-119, -28, 235),
    ["بيت مسكون"] = CFrame.new(986, 4, 63),
    ["مكان سري"] = CFrame.new(672, 4, -296),
    ["قهوة"] = CFrame.new(161, 8, 52),
    ["البداية"] = CFrame.new(-26, 4, -23),
    ["مستشفى"] = CFrame.new(-309, 4, 71),
    ["فوق الجسر"] = CFrame.new(-589, 141, -59),
    ["مكان الكهرباء"] = CFrame.new(179, 4, -464),
    ["جوه الأرض"] = CFrame.new(0, 4, -495),
    ["جوه الأرض 2"] = CFrame.new(-343, 4, -613),
    ["جوه الأرض 3"] = CFrame.new(505, -75, 143),
    ["جزيرة 1"] = CFrame.new(-1925, 23, 127),
    ["مطار"] = CFrame.new(310, 5, 31),
    ["البيوت"] = CFrame.new(182, 4, 150),
    ["نص الشارع"] = CFrame.new(63, 35, 410),
    ["فوق الجبل"] = CFrame.new(-670, 251, 765),
    ["فوق المدرسة"] = CFrame.new(-370, 50, 173)
}

AddButton(Main, {
    Name = " الذهاب إلى المكان",
    Callback = function()
        if selectedLocation and locations[selectedLocation] then
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = locations[selectedLocation]
        end
    end
})

local section = AddSection(Main, {"تروح عند الاعبين"})
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer

-- متغير لحفظ اللاعب المختار
local selectedPlayerName = nil

-- القائمة المنسدلة (Dropdown)
local playerNames = {}
for _, player in ipairs(Players:GetPlayers()) do
    table.insert(playerNames, player.Name)
end

AddDropdown(Main, {
    Name = "اختر لاعب",
    Options = playerNames,
    Default = playerNames[1] or "لا يوجد لاعبين",
    Callback = function(selected)
        selectedPlayerName = selected
    end
})

-- زر تطبيق
AddButton(Main, {
    Name = "تطبيق النقل للاعب",
    Callback = function()
        if not selectedPlayerName then
            warn("لم يتم اختيار لاعب.")
            return
        end
        local targetPlayer = Players:FindFirstChild(selectedPlayerName)
        if targetPlayer and targetPlayer.Character and targetPlayer.Character:FindFirstChild("HumanoidRootPart") then
            local targetPosition = targetPlayer.Character.HumanoidRootPart.Position
            local localRoot = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart")
            if localRoot then
                localRoot.CFrame = CFrame.new(targetPosition)
                print("تم الانتقال إلى: " .. selectedPlayerName)
            end
        else
            warn("اللاعب غير موجود أو لا يملك شخصية.")
        end
    end
})

local Main = MakeTab({
    Name = "السياره",
    Image = "rbxassetid://10709789810",
    TabTitle = false
})

-- إنشاء لابل لعرض عدد السيارات
local vehicleCountLabel = AddTextLabel(Main, " عدد السيارات: جاري التحميل...")

-- دالة لتحديث عدد السيارات
local function updateVehicleCount()
    local vehicleFolder = workspace:FindFirstChild("Vehicles")
    local count = 0

    if vehicleFolder then
        for _, obj in ipairs(vehicleFolder:GetChildren()) do
            if obj:IsA("Model") then
                count += 1
            end
        end
    end

    vehicleCountLabel.Text = " عدد السيارات: " .. count
end

-- تحديث مبدأي عند تشغيل السكربت
updateVehicleCount()

-- التحديث عند تغيير أي شيء داخل مجلد السيارات
local vehicleFolder = workspace:FindFirstChild("Vehicles")
if vehicleFolder then
    vehicleFolder.ChildAdded:Connect(updateVehicleCount)
    vehicleFolder.ChildRemoved:Connect(updateVehicleCount)
end



AddButton(Main, {
    Name = "🚨 حذف جميع السيارات",
    Callback = function()
        local ofnawufn = false

        if ofnawufn == true then
            return
        end
        ofnawufn = true

        local cawwfer = "MilitaryBoatFree"
        local oldcfffff = game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame
        game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(1754, -2, 58)
        wait(0.3)

        local args = {
            [1] = "PickingBoat",
            [2] = cawwfer
        }

        game:GetService("ReplicatedStorage").RE:FindFirstChild("1Ca1r"):FireServer(unpack(args))
        wait(1)

        local wrinfjn
        for _, errb in pairs(game.workspace.Vehicles[game.Players.LocalPlayer.Name.."Car"]:GetDescendants()) do
            if errb:IsA("VehicleSeat") then
                wrinfjn = errb
            end
        end

        repeat
            if game.Players.LocalPlayer.Character.Humanoid.Health == 0 then return end
            if game.Players.LocalPlayer.Character.Humanoid.Sit == true then
                if not game.Players.LocalPlayer.Character.Humanoid.SeatPart == wrinfjn then
                    game.Players.LocalPlayer.Character.Humanoid.Sit = false
                end
            end
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = wrinfjn.CFrame
            task.wait()
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = wrinfjn.CFrame + Vector3.new(0,1,0)
            task.wait()
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = wrinfjn.CFrame + Vector3.new(0,-1,0)
            task.wait()
        until game.Players.LocalPlayer.Character.Humanoid.SeatPart == wrinfjn

        for _, wifn in pairs(game.workspace.Vehicles[game.Players.LocalPlayer.Name.."Car"]:GetDescendants()) do
            if wifn.Name == "PhysicalWheel" then
                wifn:Destroy()
            end
        end

        local FLINGED = Instance.new("BodyThrust", game.workspace.Vehicles[game.Players.LocalPlayer.Name.."Car"].Chassis.Mass) 
        FLINGED.Force = Vector3.new(50000, 0, 50000) 
        FLINGED.Name = "SUNTERIUM HUB FLING"
        FLINGED.Location = game.workspace.Vehicles[game.Players.LocalPlayer.Name.."Car"].Chassis.Mass.Position

        for _, wvwvwasc in pairs(game.workspace.Vehicles:GetChildren()) do
            for _, ascegr in pairs(wvwvwasc:GetDescendants()) do
                if ascegr.Name == "VehicleSeat" then
                    local targetcar = ascegr
                    local tet = Instance.new("BodyVelocity", game.workspace.Vehicles[game.Players.LocalPlayer.Name.."Car"].Chassis.Mass)
                    tet.MaxForce = Vector3.new(math.huge,math.huge,math.huge)
                    tet.P = 1250
                    tet.Velocity = Vector3.new(0,0,0)
                    tet.Name = "#mOVOOEPF$#@F$#GERE..>V<<<<EW<V<<W"
                    for m=1,25 do
                        local pos = {x=0, y=0, z=0}
                        pos.x = targetcar.Position.X
                        pos.y = targetcar.Position.Y
                        pos.z = targetcar.Position.Z
                        pos.x = pos.x + targetcar.Velocity.X / 2
                        pos.y = pos.y + targetcar.Velocity.Y / 2
                        pos.z = pos.z + targetcar.Velocity.Z / 2
                        if pos.y <= -200 then
                            game.workspace.Vehicles[game.Players.LocalPlayer.Name.."Car"].Chassis.Mass.CFrame = CFrame.new(0,1000,0)
                        else
                            game.workspace.Vehicles[game.Players.LocalPlayer.Name.."Car"].Chassis.Mass.CFrame = CFrame.new(Vector3.new(pos.x,pos.y,pos.z))
                            task.wait()
                            game.workspace.Vehicles[game.Players.LocalPlayer.Name.."Car"].Chassis.Mass.CFrame = CFrame.new(Vector3.new(pos.x,pos.y,pos.z)) + Vector3.new(0,-2,0)
                            task.wait()
                            game.workspace.Vehicles[game.Players.LocalPlayer.Name.."Car"].Chassis.Mass.CFrame = CFrame.new(Vector3.new(pos.x,pos.y,pos.z)) * CFrame.new(0,0,2)
                            task.wait()
                            game.workspace.Vehicles[game.Players.LocalPlayer.Name.."Car"].Chassis.Mass.CFrame = CFrame.new(Vector3.new(pos.x,pos.y,pos.z)) * CFrame.new(2,0,0)
                            task.wait()
                        end
                        task.wait()
                    end
                end
            end
        end

        task.wait()
        local args = {
            [1] = "DeleteAllVehicles"
        }

        game:GetService("ReplicatedStorage").RE:FindFirstChild("1Ca1r"):FireServer(unpack(args))
        game.Players.LocalPlayer.Character.Humanoid.Sit = false
        wait()
        local tet = Instance.new("BodyVelocity", game.Players.LocalPlayer.Character.HumanoidRootPart)
        tet.MaxForce = Vector3.new(math.huge,math.huge,math.huge)
        tet.P = 1250
        tet.Velocity = Vector3.new(0,0,0)
        tet.Name = "#mOVOOEPF$#@F$#GERE..>V<<<<EW<V<<W"
        wait(0.1)
        for m=1,2 do 
            task.wait()
            game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = oldcfffff
        end
        wait(1)
        game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = oldcfffff
        wait()
        game.Players.LocalPlayer.Character.HumanoidRootPart:FindFirstChild("#mOVOOEPF$#@F$#GERE..>V<<<<EW<V<<W"):Destroy()
        wait(0.2)
        ofnawufn = false
    end
})





-- المتغيرات
local CarSpeed = 0
local TurboStage = 1
local DriftPower = 1

-- المرجع إلى الريموتات
local RE = game:GetService("ReplicatedStorage"):WaitForChild("RE")
local carRemote = RE:WaitForChild("1Player1sCa1r")


-- TextBox: سرعة السيارة
AddTextBox(Main, {
    Name = " سرعة السيارة",
    Default = "100",
    PlaceholderText = "أدخل السرعة",
    ClearText = true,
    Callback = function(val)
        local n = tonumber(val)
        if n then
            CarSpeed = n
        end
    end
})

-- TextBox: التيربو
AddTextBox(Main, {
    Name = " مرحلة التيربو (1 إلى 3)",
    Default = "1",
    PlaceholderText = "مثال: 2",
    ClearText = true,
    Callback = function(val)
        local n = tonumber(val)
        if n and n >= 1 and n <= 3 then
            TurboStage = n
        end
    end
})

-- TextBox: قوة التفحيط
AddTextBox(Main, {
    Name = " قوة التفحيط",
    Default = "1",
    PlaceholderText = "مثال: 5",
    ClearText = true,
    Callback = function(val)
        local n = tonumber(val)
        if n then
            DriftPower = n
        end
    end
})

-- زر لتطبيق الإعدادات
AddButton(Main, {
    Name = " تطبيق ",
    Callback = function()
        -- سرعة السيارة
        carRemote:FireServer("PlayerGiveSpeedLower", CarSpeed)

        -- التيربو
        carRemote:FireServer("TurboStage", TurboStage)

        -- قوة التفحيط
        carRemote:FireServer("DriftPower", DriftPower)
    end
})




AddSection(Main, {"اخرى"})

AddSection(Main, {"اغاني سياراة مجانيه"})

local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Players = game:GetService("Players")
local player = Players.LocalPlayer

local soundVolume = 100
local textBoxSoundID = nil

-- 🔊 دالة لتشغيل الصوت على السيارة فقط
local function Play_OnCar(ID)
	if not ID then return end
	local carEvent = ReplicatedStorage:FindFirstChild("RE"):FindFirstChild("1Player1sCa1r")
	if carEvent then
		carEvent:FireServer("CarMusic", tonumber(ID), math.clamp(soundVolume, 0, 400) / 100)
		print("✅ تم تشغيل الصوت على السيارة: " .. ID)
	else
		warn("❌ حدث تشغيل السيارة غير موجود")
	end
end

-- 🎵 TextBox لإدخال كود الأغنية
AddTextBox(Main, {
	Name = "ضع كود الاغنية",
	PlaceholderText = "هنا",
	ClearText = true,
	Callback = function(Value)
		local id = tonumber(Value)
		if id then
			textBoxSoundID = id
			Play_OnCar(id) -- 🔁 تشغيل فورًا على السيارة
		else
			warn("❌ أدخل رقم ID صحيح فقط")
			textBoxSoundID = nil
		end
	end
})



AddButton(Main, {
    Name = "سبام سيارات",
    Callback = function()
        task.spawn(function()
            local models = {"SchoolBus", "350Z", "P50", "Tank"}
            while true do
                for _, model in ipairs(models) do
                    game:GetService("ReplicatedStorage").RE["1Ca1r"]:FireServer("PickingCar", model)
                    task.wait(0.5)
                end
            end
        end)
    end
})

AddButton(Main, {
    Name = "سبام حصان",
    Callback = function()
        task.spawn(function()
            while true do
                game:GetService("ReplicatedStorage").RE["1Ca1r"]:FireServer("PickingCar", "Horse")
                task.wait(0.5)
            end
        end)
    end
})



local Main = MakeTab({
    Name = "البيت",
    Image = "rbxassetid://10723407389",
    TabTitle = false
})


AddButton(Main, {
    Name = "سبام فتح وقفل",
    Callback = function()
        spawn(function()
            while task.wait() do
                local args = {
                    [1] = "GarageDoor"
                }
                game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1Player1sHous1e"):FireServer(unpack(args))
            end
        end)
    end
})

AddButton(Main, {
    Name = "فتح وغلق شباك",
    Callback = function()
        local args = {
            [1] = "Curtains"
        }
        game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1Player1sHous1e"):FireServer(unpack(args))
    end
})

AddButton(Main, {
    Name = "قفل باب بيت",
    Callback = function()
        local args = {
            [1] = "LockDoors"
        }
        game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1Player1sHous1e"):FireServer(unpack(args))
    end
})

AddDropdown(Main, {
    Name = "التحكم في البيت",
    Default = "اختر رقم البيت",
    Options = {"1","2","3","4","5","6","7","8","9","10","11","12","13","14","15","16","17","18","19","20","21","22","23","24"},
    Callback = function(Value)
        _G.SelectedHouse = Value
    end
})

AddButton(Main, {
    Name = "اخذ صلاحيات البيت",
    Callback = function()
        if _G.SelectedHouse then
            local args = {
                [1] = "GivePermissionLoopToServer",
                [2] = game:GetService("Players").LocalPlayer,
                [3] = tonumber(_G.SelectedHouse)
            }
            local success, error = pcall(function()
                game:GetService("ReplicatedStorage").RE:FindFirstChild("1Playe1rTrigge1rEven1t"):FireServer(unpack(args))
            end)
            if not success then
                warn("Error:", error)
            end
        end
    end
})

local section = AddSection(Main, {"اخرى"})

AddTextBox(Main, {
    Name = "ادخل كود صوت البيت",
    Default = "",
    PlaceholderText = "ادخل كود الأغنية للبيت",
    ClearText = true,
    Callback = function(Value)
        if not _G.SelectedHouse then
            warn("يرجى اختيار رقم البيت أولاً من الدروب داون.")
            return
        end

        local houseRemote = game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1Player1sHous1e")
        -- أولاً إرسال أمر لتشغيل الموسيقى للبيت المحدد
        houseRemote:FireServer("PlayHouseMusic", tonumber(_G.SelectedHouse))

        -- ثم إرسال الكود الصوتي للبيت (كود الأغنية)
        houseRemote:FireServer("HouseMusicCode", Value)
    end
})


AddButton(Main, {
    Name = "لون بيتك",
    Callback = function()
        local colors = {
            Color3.fromRGB(255, 0, 0),
            Color3.fromRGB(255, 127, 0),
            Color3.fromRGB(255, 255, 0),
            Color3.fromRGB(0, 255, 0),
            Color3.fromRGB(0, 0, 255),
            Color3.fromRGB(75, 0, 130),
            Color3.fromRGB(148, 0, 211)
        }
        local remote = game:GetService("ReplicatedStorage"):WaitForChild("RE"):WaitForChild("1Player1sHous1e")
        while true do
            for _, color in ipairs(colors) do
                local args = {
                    [1] = "ColorPickHouse",
                    [2] = color
                }
                remote:FireServer(unpack(args))
                wait(0.5)
            end
        end
    end
})

AddToggle(Main, {
    Name = "الغاء حظر عن بيت",
    Default = false,
    Callback = function(state)
        isUnbanActive = state
        if isUnbanActive then
            startAutoUnban()
        end
    end
})

function startAutoUnban()
    while isUnbanActive do
        for i, v in pairs(game:GetService("Workspace"):WaitForChild("001_Lots"):GetDescendants()) do
            if v.Name == "BannedBlock1" or v.Name == "BannedBlock2" or v.Name == "BannedBlock3" or v.Name == "BannedBlock4" or v.Name == "BannedBlock5" or v.Name == "BannedBlock6" or v.Name == "BannedBlock7" or v.Name == "BannedBlock11" or v.Name == "BannedBlock12" or v.Name == "BannedBlock13" or v.Name == "BannedBlock14" or v.Name == "BannedBlock15" or v.Name == "BannedBlock16" or v.Name == "BannedBlock17" or v.Name == "BannedBlock18" or v.Name == "BannedBlock19" or v.Name == "BannedBlock20" or v.Name == "BannedBlock21" or v.Name == "BannedBlock22" or v.Name == "BannedBlock23" or v.Name == "BannedBlock24" or v.Name == "BannedBlock30" or v.Name == "BannedBlock31" or v.Name == "BannedBlock32" or v.Name == "BannedBlock33" or v.Name == "BannedBlock34" or v.Name == "BannedBlock35" then
                v:Destroy()
            end
        end
        wait(1)
    end
end
