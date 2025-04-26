---
theme: default
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

<div class="flex justify-center">
    <img
        class="w-30 mx-8"
        src="/python.png"
        alt=""
    />
    <img
        class="w-30 mx-8"
        src="/discord.png"
        alt=""
    />
</div>

<!-- # 上竣
- AI 新創公司，全端開發 (Vue + Python)
- 生物研究 -> 為了加速實驗數據分析而接觸 Python
  - 資料處理
  - 自動化工具
- Discord 遊戲群組的需求 -> 開始研究 Discord BOT -->

---

# 大綱
- Discord BOT
- 指令
- UI 元件互動
- 狀態提示


---
class: flex justify-center items-center

---

# Discord BOT 簡介

---
layout: two-cols
---

# 最基本的 Discord BOT

<div class="mr-8">

````md magic-move {lines: true}
```python
import discord

intents = discord.Intents.default()
intents.message_content = True
client = discord.Client(intents=intents)

@client.event
async def on_message(message):
    if message.author == client.user:
        return

    if message.content == 'ping':
        await message.channel.send('pong')

client.run('token')
```
````

</div>

::right::

<br>
<br>

<v-click>

````md magic-move {lines: true}
```python
import discord
from discord.ext import commands

intents = discord.Intents.default()
intents.message_content = True
bot = commands.Bot(command_prefix='', intents=intents)

@bot.command()
async def ping(ctx):
    await ctx.send('pong')

bot.run('token')
```
````

</v-click>

<br>
<br>
<br>
<br>
<br>

<v-click>
<div class="text-center">

### 指令 (Command)

</div>
</v-click>

<!-- 
- ## Demo
  - ### 輸入：ping
  - ### 輸出：pong
- 這是一個最簡單的 discord BOT，長這樣
  - 稍微解釋
  - 提到指令
- [click] 如果你的 discord BOT 大部分功能都是這類的指令，可以考慮使用 discord.py 提供的 bot 指令框架，讓我們可以更容易去開發，程式碼長這樣
  - 稍微解釋
  - prefix 的目的 (有可能會不小心觸發，所以通常會在前面加一個前墜)

當然，如果是要針對其他事件，例如修改訊息，那還是要回去使用第一個寫法就是了
另外，補充一下，其實還有一個 cog 的寫法，但這邊礙於時間的關係就不多做介紹了。
接下來的範例會以 bot 指令框架的寫法為主 -->

---
class: flex justify-center items-center
---

# 指令 Command

---
layout: two-cols
---

# 基本的 command

````md magic-move {lines: true}
```python {*|8-10}
import discord
from discord.ext import commands

intents = discord.Intents.default()
intents.message_content = True
bot = commands.Bot(command_prefix='', intents=intents)

@bot.command()
async def greet(ctx):
    await ctx.send('hello')

bot.run('token')
```

```python {8-10}
import discord
from discord.ext import commands

intents = discord.Intents.default()
intents.message_content = True
bot = commands.Bot(command_prefix='', intents=intents)

@bot.command()
async def greet(ctx, name):
    await ctx.send(f'hello {name}')

bot.run('token')
```

```python {8-10}
import discord
from discord.ext import commands

intents = discord.Intents.default()
intents.message_content = True
bot = commands.Bot(command_prefix='', intents=intents)

@bot.command()
async def greet(ctx, name1, name2):
    await ctx.send(f'hello {name1} and {name2}')

bot.run('token')
```
````

<!--
- 根據上一個範例，這邊只要輸入 greet，就會得到 hello
- ## Demo 1
  - `$greet`
  - ### 輸入：greet 小明 小王
  - ### 輸出：hello 小明 小王
-->


---
layout: two-cols
---

# 斜線指令 Slash Command

更高級的指令

- 有自己獨立的界面
- 有說明引導
- 可以設定選項
- 可以設定範圍

::right::

<br>
<br>
<br>

<img
    src="https://firebasestorage.googleapis.com/v0/b/images-7e754.appspot.com/o/ithome_2024%2F07_default.png?alt=media&token=9d6134cf-6097-43ad-a7d6-49f1ee13bb7f"
    alt=""
/>



---
layout: two-cols-header
---

# 斜線指令 Slash Command

::left::

- 可以設定選項
  - Choice
  - Literal
  - Enum

![](https://firebasestorage.googleapis.com/v0/b/images-7e754.appspot.com/o/ithome_2024%2F10_literal_02-2.png?alt=media&token=38e5561f-647d-452b-aae6-337e5931d8c0)

::right::

````md magic-move {lines: true}
```python
from discord.app_commands import Choice


@bot.tree.command(description="點餐")
@app_commands.choices(tea=[
    Choice(name='綠茶', value='green_tea'),
    Choice(name='紅茶', value='black_tea'),
    Choice(name='奶茶', value='milk_tea'),
])
@app_commands.choices(size=[
    Choice(name='小杯', value='small'),
    Choice(name='中杯', value='medium'),
    Choice(name='大杯', value='large'),
])
async def order(
    interaction: discord.Interaction,
    tea: Choice[str],
    size: Choice[str],
):
    await interaction.response.send_message(
        f"點餐結果：{tea.name}{size.name}"
    )
```
````

<!--
- 這邊有省略掉一些東西
- 有三種寫法
- ## Demo 2-1 早安午安
  - `/goodmorning`
  - `/晚安`
- ## Demo 2-2 order
  - `/order`
-->

---
layout: two-cols-header
---

# 斜線指令 Slash Command

::left::

- 可以設定選項
  - Choice
  - Literal
  - Enum

<div class="mr-4">

````md magic-move {lines: true}
```python
from typing import Literal


@bot.tree.command(description="點餐")
async def order(
    interaction: discord.Interaction,
    tea: Literal["綠茶", "紅茶", "奶茶"],
    size: Literal["大杯", "中杯", "小杯"],
):
    await interaction.response.send_message(
        f"點餐結果：{tea}{size}"
    )
```
````

</div>

::right::

````md magic-move {lines: true}
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

@bot.tree.command(description="點餐")
async def order(
    interaction: discord.Interaction,
    tea: Tea,
    size: Size,
):
    await interaction.response.send_message(
        f"點餐結果：{tea.value}{size.value}"
    )
```
````

<!--
- 其他兩種寫法
- 不 Demo
-->

---
layout: two-cols
---

# 斜線指令 Slash Command

- 可以設定範圍

<div class=mr-8>

````md magic-move {lines: true}
```python
from typing import Literal


@bot.tree.command(description="點餐")
async def order(
    interaction: discord.Interaction,
    tea: Literal["綠茶", "紅茶", "奶茶"],
    size: Literal["大杯", "中杯", "小杯"],
):
    await interaction.response.send_message(
        f"點餐結果：{tea}{size}"
    )
```

```python
from typing import Literal


@bot.tree.command(description="點餐")
async def order(
    interaction: discord.Interaction,
    tea: Literal["綠茶", "紅茶", "奶茶"],
    size: Literal["大杯", "中杯", "小杯"],
    number: Range[int, 1, 10],
):
    await interaction.response.send_message(
        f"點餐結果：{tea}{size} {number}杯"
    )
```
````

</div>

::right::

<br>
<br>
<br>

<v-click>

![](https://firebasestorage.googleapis.com/v0/b/images-7e754.appspot.com/o/ithome_2024%2F10_range_01.png?alt=media&token=1daf9ae1-97b0-441b-b2c8-54787328282d)

</v-click>

<v-click>

![](https://firebasestorage.googleapis.com/v0/b/images-7e754.appspot.com/o/ithome_2024%2F10_range_02.png?alt=media&token=8cd91346-aad7-4e0e-9490-32983a7a15fa)

</v-click>

<!--
- 有些東西不適合/沒辦法全部列出來
- ## Demo 2-3
  - /order_v2
  - ### 正確使用
  - ### 錯誤使用 (有錯誤提示)
-->

---

# User Command 與 Message Command


````md magic-move {lines: true}
```python {*|1,2,12,13|1,5,12,16|*}
# User Command
@bot.tree.context_menu(name="顯示加入時間")
async def show_join_date(
    interaction: discord.Interaction,
    member: discord.Member
):
    await interaction.response.send_message(
        f"{member} joined at {discord.utils.format_dt(member.joined_at)}",
        ephemeral=True,
    )

# Message Command
@bot.tree.context_menu(name="檢舉內容")
async def report_message(
    interaction: discord.Interaction,
    message: discord.Message
):
    await interaction.response.send_message(
        f"收到 {message.author.mention} 對這個訊息的檢舉",
        ephemeral=True,
    )
```
````

<!--
- 只有一個參數，選項是 User 或 Message
- command -> context_menu
- ## Demo 3-1
  - show_join_date
- ## Demo 3-2
  - report_message
-->

---
class: flex justify-center items-center

---

# UI 元件互動

---
layout: two-cols
---

# 按鈕

<br>

<img
    src="https://firebasestorage.googleapis.com/v0/b/images-7e754.appspot.com/o/ithome_2024%2F14_view_01.png?alt=media&token=778092b7-a3bf-48a0-928d-b3a60d20c246"
    alt=""
/>

::right::

<br>
<br>
<br>

<v-click>

````md magic-move {lines: true}
```python {*|9-14}
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

```python {9-18|9,10,16}
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



<!-- 需要注意的是，只能是 interaction -->

---
layout: two-cols
---

# 按鈕
另一種寫法：class

::right::

````md magic-move {lines: true}
```python {*|9-19}
import discord
from discord.ext import commands


intents = discord.Intents.default()
intents.message_content = True
bot = commands.Bot(command_prefix="", intents=intents)

class MyView(discord.ui.View):
    def __init__(self):
        super().__init__()
        self.add_item(
            discord.ui.Button(label="我是按鈕")
        )

@bot.command()
async def ping(ctx: commands.Context):
    view = MyView()
    await ctx.send("點擊下方按鈕", view=view)

bot.run("token")
```

```python {9-26}
import discord
from discord.ext import commands


intents = discord.Intents.default()
intents.message_content = True
bot = commands.Bot(command_prefix="", intents=intents)

class MyView(discord.ui.View):
    def __init__(self):
        super().__init__()
    
    @discord.ui.button(label="我是按鈕")
    async def on_click(
        self,
        interaction: discord.Interaction,
        button: discord.ui.Button
    ):
        await interaction.response.send_message(
            "點擊了按鈕"
        )

@bot.command()
async def ping(ctx: commands.Context):
    view = MyView()
    await ctx.send("點擊下方按鈕", view=view)

bot.run("token")
```
````

<!-- - ## Demo 4-1
  - `$button`
  - 可以反覆觸發
- ## Demo 4-2
  - `$button_v2`
  - 樣式調整、編輯訊息 -->

---

# 按鈕

小技巧

- 編輯訊息
- 樣式

<div class="flex justify-center">
<img
    class="w-150"
    src="https://firebasestorage.googleapis.com/v0/b/images-7e754.appspot.com/o/ithome_2024%2F14_btn_styles.png?alt=media&token=264b941e-85cc-44ae-b996-92110bcdbb28"
    alt=""
/>
</div>

<br>

<div class="flex justify-center">
<span class="opacity-30 text-xs">https://discord.com/developers/docs/components/reference#button-object</span>
</div>

<!-- 後續可以串接的功能很多，大家可以自由發揮，這邊舉幾個例子：
1. 確認/取消按鈕，來決定是不是真的要執行
2. 提供多個選項讓使用者選擇，就像 midjourney 
3. 如果後續有要再繼續追問，可以用編輯訊息的方式

- ## Demo 4-1
  - `$button`
    - 可以反覆觸發
  - `$button_v2`
    - 樣式調整、編輯訊息 
-->

---
layout: TwoCols57
---

# 下拉選單

<br>

- String Select

<br>

<div class="mr-4">

![](https://firebasestorage.googleapis.com/v0/b/images-7e754.appspot.com/o/ithome_2024%2F15_select_02.png?alt=media&token=869dde9a-a56e-499e-80fb-6a912a8d64be)

</div>

::right::

````md magic-move {lines: true}
```python {*|8-24}
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

<!--
- 按鈕有時候不適合放太多，這時候可以考慮改用下拉式選單
-->

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

<!--
- 除了自己定義選項之外，discord.py 也有幾個幫忙定義好選項的下拉式選單，包含...
- ## Demo 4-2
  - `$select`
    - 可以反覆觸發
  - `$select_v2`
    - 各種選單都有 
-->

---
layout: TwoCols57
---

# 互動視窗 Modal

<br>

<div class="w-70">

![](https://firebasestorage.googleapis.com/v0/b/images-7e754.appspot.com/o/ithome_2024%2F17_demo_01.png?alt=media&token=486349db-6af1-4c0e-b97d-750cf51edd0f)

</div>

<v-click>

<div class="w-70">

![](https://firebasestorage.googleapis.com/v0/b/images-7e754.appspot.com/o/ithome_2024%2F17_demo_02.png?alt=media&token=030dd0a9-7d93-4727-932d-0158ca58cf2c)

</div>

</v-click>

::right::

<br>
<br>
<br>

````md magic-move {lines: true}
```python
import discord


class Questionnaire(discord.ui.Modal, title="Questionnaire Response"):
    name = discord.ui.TextInput(label="Name")
    answer = discord.ui.TextInput(
        label="Answer",
        style=discord.TextStyle.paragraph
    )

    async def on_submit(self, interaction: discord.Interaction):
        await interaction.response.send_message(
            f"Thanks for your response, {self.name}!",
            ephemeral=True
        )

@bot.tree.command()
async def ping(interaction: discord.Interaction):
    await interaction.response.send_modal(Questionnaire())
```
````

<!-- 
- 展示 Modal 範例 + 說明 code
  - label (str)：標籤，也就是這個 Text Input 的標題
  - style (discord.TextStyle)：風格，只有兩種，分成 short 和 paragraph (long) (可以參考文件)
  - placeholder (str)：提示文字
  - default (str)：預設值
  - required (bool)：是否為必填 (預設為 True)
  - min_length (int)：至少要輸入的字串長度 (0 到 4000)
  - max_length (int)：至多能輸入的字串長度 (1 到 4000)
  - row (int)：排序編號。一個 Modal 至多只能有 5 個 Text Input，所以 row 必須介於 0 到 4 之間，而且不能重複。(沒設定的話，就會依照程式碼的順序排列)
- [click] 送出之後就會觸發 `on_submit`
- 解釋 ephemeral ( *əˈfem(ə)rəl* )
- ## Demo 4-3
  - `/午餐問卷調查`
-->

---
layout: two-cols
---

# 狀態提示

- 提示使用者 Discord BOT 正在處理中
  - 正在輸入...
  - 正在思考...

<div class="w-100">

![](https://firebasestorage.googleapis.com/v0/b/images-7e754.appspot.com/o/ithome_2024%2F16_typing.png?alt=media&token=be2056eb-da77-47c9-bbba-4dd4ac34bbd7)

</div>

<br>

<div class="w-100">

![](https://firebasestorage.googleapis.com/v0/b/images-7e754.appspot.com/o/ithome_2024%2F30_defer_01.png?alt=media&token=843a0b6c-c719-4276-90d7-c88d66905dc9)

</div>

::right::

<br>
<br>
<br>

<v-click>

````md magic-move {lines: true}
```python
@bot.command()
async def ping(ctx: commands.Context):
    await ctx.typing()
    await asyncio.sleep(5)
    await ctx.send('pong')
```
````

<br>

````md magic-move {lines: true}
```python
@bot.command()
async def ping(ctx: commands.Context):
    async with ctx.typing():
        await asyncio.sleep(5)
    await ctx.send('pong')
```
````

</v-click>

<br>

<v-click>

````md magic-move {lines: true}
```python
@app_commands.command()
async def exchange(self, interaction: Interaction):
    await interaction.response.defer()
    await asyncio.sleep(5)
    await interaction.followup.send("計算完成！")
```
````

</v-click>

<!--
- 平常在使用 APP 或網頁會有 Loading 畫面或提示，Discord BOT 也可以做到類似的狀態提示
  - 有兩種，
- 如果是 Interaction
- ## Demo 5
  - `$typing`
  - `/思考`
-->

---

<br>
<br>

<div class="flex justify-center">

# Thank you

</div>

<br>

<div class="flex justify-center">
    <img
        class="w-70"
        src="/qrcode.png"
        alt=""
    />
</div>

<br>

<div class="flex justify-center">

## 簡報連結

</div>