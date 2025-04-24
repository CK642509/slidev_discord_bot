---
theme: default
background: https://cdn3.emoji.gg/emojis/9355-discordpy.png
title: 用 Python 打造高互動性的 Discord BOT
info: |
  ## 關於這份簡報
  嗨！我是上竣~
  我目前在一間 AI 新創公司從全端開發的工作，負責將公司所開發的 AI 技術包裝成各個產品。最初是為了要加速生物實驗的數據分析才開始接觸程式，沒想到最後因厭倦做實驗而轉行，誤打誤撞地踏上了全端開發之路。

  本次演講將介紹如何使用 discord.py 開發 Discord Bot，內容將涵蓋指令、按鈕、下拉選單與彈出視窗等多種互動方式，展示如何將 Discord 打造成簡單且高效的前端介面。透過實際範例，探索 Bot 在不同應用場景中的可能性，幫助開發者設計出更具互動性與實用性的工具。

  原始檔都放在[Github](https://github.com/CK642509/slidev_discord_bot)上，歡迎查看
class: text-center
drawings:
  persist: false
transition: slide-left
mdc: true
---

# 用 Python 打造高互動性的 Discord BOT

---

# 上竣
- 非本科系 (農業化學系、生醫電資所)
- ~ 3 年年資
- AI 新創公司，全端開發 (Vue + Python)
- iThome 鐵人賽
  - 2024 Python 組 「用 Python 打造你的 Discord BOT」 佳作
  - 2023 Software Development 組 「FastAPI 開發筆記：從新手到專家的成長之路」


---
class: flex justify-center items-center

---

# Discord BOT 簡介

---

# 最基本的 Discord BOT 範例

ping pong

左邊程式碼，右邊 discord 網頁

---
class: flex justify-center items-center

---

# 指令 Command

---

# 基本的 command

---

# Slash Command

- 可以帶參數
- 基礎的範例

---
# layout: TwoColumn57
layout: two-cols-header
---

# Slash Command

- 可以幫參數設選項

::left::

<div class="mr-4">

```python
from typing import Literal

@client.tree.command(description="點餐")
async def order(
    interaction: discord.Interaction,
    tea: Literal["綠茶", "紅茶", "奶茶"],
    size: Literal["大杯", "中杯", "小杯"],
):
    await interaction.response.send_message(
        f"點餐結果：{tea}{size}"
    )
```

</div>

::right::
```python
from enum import Enum

class Tea(Enum):
    green_tea = "綠茶"
    black_tea = "紅茶"
    milk_tea = "奶茶"

class Size(Enum):
    small = "小杯"
    medium = "中杯"
    large = "大杯"

@client.tree.command(description="點餐")
async def order(
    interaction: discord.Interaction,
    tea: Tea,
    size: Size,
):
    await interaction.response.send_message(
        f"點餐結果：{tea.value}{size.value}"
    )
```

---

# Slash Command

- 可以幫參數設定範圍
- 範例

---

# User Command 與 Message Command

- 只有一個參數，選項是 User 或 Message
- 各一個範例

---
class: flex justify-center items-center

---

# UI 元件互動

---
layout: two-cols
---

# 按鈕

動畫順序要再改

<br>

<img
    src="https://firebasestorage.googleapis.com/v0/b/images-7e754.appspot.com/o/ithome_2024%2F14_view_01.png?alt=media&token=778092b7-a3bf-48a0-928d-b3a60d20c246"
    alt=""
/>

::right::

<v-click>

````md magic-move {lines: true}
```python {*|8-13|*}
import discord
from discord.ext import commands

intents = discord.Intents.default()
intents.message_content = True
bot = commands.Bot(command_prefix="", intents=intents)

@bot.command()
async def ping(ctx: commands.Context):
    view = discord.ui.View()
    btn = discord.ui.Button(label="我是按鈕")
    view.add_item(btn)
    await ctx.send("點擊下方按鈕", view=view)

bot.run("token")
```

```python {*|8,9,15|8-17|*}
import discord
from discord.ext import commands

intents = discord.Intents.default()
intents.message_content = True
bot = commands.Bot(command_prefix="", intents=intents)

async def on_click(interaction: discord.Interaction):
    await interaction.response.send_message("點擊了按鈕")

@bot.command()
async def ping(ctx: commands.Context):
    view = discord.ui.View()
    btn = discord.ui.Button(label="我是按鈕")
    btn.callback = on_click
    view.add_item(btn)
    await ctx.send("點擊下方按鈕", view=view)

bot.run("token")
```
````

</v-click>

<v-switch>
  <template #1>
    <img
      src="https://firebasestorage.googleapis.com/v0/b/images-7e754.appspot.com/o/ithome_2024%2F14_view_01.png?alt=media&token=778092b7-a3bf-48a0-928d-b3a60d20c246"
      alt=""
    />
  </template>
  <template #2>
    <img
      src="https://firebasestorage.googleapis.com/v0/b/images-7e754.appspot.com/o/ithome_2024%2F14_view_02.png?alt=media&token=29f0a389-d5d1-4187-be6a-a2989884eb0f"
      alt=""
    />
  </template>
  <template #3>
    <img
      src="https://firebasestorage.googleapis.com/v0/b/images-7e754.appspot.com/o/ithome_2024%2F14_view_03.png?alt=media&token=65ef25c0-b4a6-4e2b-a94e-17a93cf1da91"
      alt=""
    />
  </template>
</v-switch>

<!-- 需要注意的是，只能是 interaction -->

---

# 按鈕
另一種寫法：class

````md magic-move {lines: true}
```python {*|8-16|*}
import discord
from discord.ext import commands

intents = discord.Intents.default()
intents.message_content = True
bot = commands.Bot(command_prefix="", intents=intents)

class MyView(discord.ui.View):
    def __init__(self):
        super().__init__()
        self.add_item(discord.ui.Button(label="我是按鈕"))

@bot.command()
async def ping(ctx: commands.Context):
    view = MyView()
    await ctx.send("點擊下方按鈕", view=view)

bot.run("token")
```

```python {*|8-14}
import discord
from discord.ext import commands

intents = discord.Intents.default()
intents.message_content = True
bot = commands.Bot(command_prefix="", intents=intents)

class MyView(discord.ui.View):
    def __init__(self):
        super().__init__()
    
    @discord.ui.button(label="我是按鈕")
    async def on_click(self, interaction: discord.Interaction, button: discord.ui.Button):
        await interaction.response.send_message("點擊了按鈕")

@bot.command()
async def ping(ctx: commands.Context):
    view = MyView()
    await ctx.send("點擊下方按鈕", view=view)

bot.run("token")
```
````

---

# 按鈕

小技巧

- 樣式
- 編輯訊息

<img
    class="w-150"
    src="https://firebasestorage.googleapis.com/v0/b/images-7e754.appspot.com/o/ithome_2024%2F14_btn_styles.png?alt=media&token=264b941e-85cc-44ae-b996-92110bcdbb28"
    alt=""
/>
<span class="opacity-30 text-xs">https://discord.com/developers/docs/components/reference#button-object</span>


<!-- 後續可以串接的功能很多，大家可以自由發揮，這邊舉幾個例子：
1. 確認/取消按鈕，來決定是不是真的要執行
2. 提供多個選項讓使用者選擇，就像 midjourney 
3. 如果後續有要再繼續追問，可以用編輯訊息的方式 -->

---
layout: TwoCols57
---

# 下拉選單

簡單範例

::right::

````md magic-move {lines: true}
```python {*|8-24|*}
import discord
from discord.ext import commands

intents = discord.Intents.default()
intents.message_content = True
bot = commands.Bot(command_prefix="", intents=intents)

async def on_select(interaction: discord.Interaction):
    result = interaction.data["values"][0]
    await interaction.response.send_message(result)

@bot.command()
async def ping(ctx: commands.Context):
    view = discord.ui.View()
    select = discord.ui.Select(
        placeholder="選擇一個選項",
        options=[
            discord.SelectOption(label="選項1", value="1"),
            discord.SelectOption(label="選項2", value="2"),
        ],
    )
    select.callback = on_select
    view.add_item(select)
    await ctx.send("來看看 Select 吧", view=view)

bot.run("token")
```
````

---
layout: TwoCols57
---

# 下拉選單

其他種類

- User Select
- Role Select
- Mentionable Select
- Channel Select

::right::

````md magic-move {lines: true}
```python {*|15-21}
import discord
from discord.ext import commands

intents = discord.Intents.default()
intents.message_content = True
bot = commands.Bot(command_prefix="", intents=intents)

async def on_select(interaction: discord.Interaction):
    result = interaction.data["values"][0]
    await interaction.response.send_message(result)

@bot.command()
async def ping(ctx: commands.Context):
    view = discord.ui.View()
    select = discord.ui.Select(
        placeholder="選擇一個選項",
        options=[
            discord.SelectOption(label="選項1", value="1"),
            discord.SelectOption(label="選項2", value="2"),
        ],
    )
    select.callback = on_select
    view.add_item(select)
    await ctx.send("來看看 Select 吧", view=view)

bot.run("token")
```

```python {15-18}
import discord
from discord.ext import commands

intents = discord.Intents.default()
intents.message_content = True
bot = commands.Bot(command_prefix="", intents=intents)

async def on_select(interaction: discord.Interaction):
    result = interaction.data["values"][0]
    await interaction.response.send_message(result)

@bot.command()
async def ping(ctx: commands.Context):
    view = discord.ui.View()
    select = discord.ui.UserSelect(
        placeholder="選擇一個使用者",
        min_values=0
    )
    select.callback = on_select
    view.add_item(select)
    await ctx.send("來看看 Select 吧", view=view)

bot.run("token")
```
````

---

# Modal

簡單範例
限制

---

# 思考中 輸入中


---
class: flex justify-center items-center

---

# Thank you