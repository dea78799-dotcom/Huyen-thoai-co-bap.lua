-- Tải thư viện Rayfield GUI
local Rayfield = loadstring(game:HttpGet('https://sirius.menu/rayfield'))()

-- Tạo cửa sổ GUI
local Window = Rayfield:CreateWindow({
   Name = "Huyền thoại cơ bắp",
   LoadingTitle = "Đang tải giao diện...",
   LoadingSubtitle = "AI HACK",
   ConfigurationSaving = {
      Enabled = true,               -- Bật tự động lưu cấu hình
      FolderName = "HuyenThoaiCoBap",-- Thư mục lưu cấu hình
      FileName = "Config"
   },
   Discord = {
      Enabled = false,
      Invite = "noinvitelink",
      RememberJoins = true
   },
   KeySystem = false
})

-- Tạo Tab "Farm"
local FarmTab = Window:CreateTab("Farm", 4483362458)

-- Khai báo biến quản lý trạng thái bật/tắt
local autoRep = false

-- Tạo nút bật/tắt (Toggle)
local Toggle = FarmTab:CreateToggle({
   Name = "Tự động tập luyện (nâng tạ)",
   CurrentValue = false,
   Flag = "AutoRepToggle", -- Flag dùng để lưu và tải lại cấu hình
   Callback = function(Value)
      autoRep = Value
      
      if autoRep then
         -- Gửi thông báo bắt đầu tập luyện
         Rayfield:Notify({
            Title = "Thông Báo",
            Content = "Đã BẬT tự động tập luyện (30 lần/giây)!",
            Duration = 3,
            Image = 4483362458,
         })
         
         -- Chạy vòng lặp tập luyện 30 lần / 1 giây
         task.spawn(function()
            while autoRep do
               local player = game:GetService("Players").LocalPlayer
               local event = player and player:FindFirstChild("muscleEvent")
               
               if event then
                  event:FireServer("rep")
               else
                  -- Thông báo lỗi nếu không tìm thấy muscleEvent
                  Rayfield:Notify({
                     Title = "Lỗi!",
                     Content = "Không tìm thấy 'muscleEvent' trong Player!",
                     Duration = 4,
                     Image = 4483362458,
                  })
                  break -- Dừng vòng lặp nếu gặp lỗi
               end
               
               task.wait(1 / 30) -- Chạy 30 lần mỗi giây
            end
         end)
      else
         -- Thông báo khi tắt
         Rayfield:Notify({
            Title = "Thông Báo",
            Content = "Đã TẮT tự động tập luyện!",
            Duration = 3,
            Image = 4483362458,
         })
      end
   end,
})

-- Tải lại cấu hình đã lưu từ lần trước
Rayfield:LoadConfiguration()
