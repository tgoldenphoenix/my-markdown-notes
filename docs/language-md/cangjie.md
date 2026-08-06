# Cangjie input method

When using cangjie to type simplified characters, the codes seems inconsistent. basic characters don't seem to work like simplified versions of 學, 話 etc.  
Like 学. The code should be 火月弓木(FBND) according to cangjie rules but in Windows 11, the correct code is 戈月弓木(IBND). It is actually not too bad as you can easily google the correct code. I found few words that even google and cangjie guide books didn't give me the correct answers.

To google the code: type a character in traditional font and follow it with 簡體倉頡碼 in search box.

## Decomposition Rules

Single-unit characters if includes more than 4 radicals: F3+L

Double-unit characters: F+L // F2+L

---

Triple-unit characters

- Normally là:
  - 1st, last // 1st, last // last (`F+L // F+L // L`)
  - or 1st // 1st, last // last (`F // F+L // L`)
- We take the first + last radical of the last unit only when the **second unit** takes one radical => `F+L // F // F+L`
  - 實 (十 // 田十 // 金) => In this character, the 2nd unit takes two radicals (not one), so we only take the last radical of the third unit
  - 瑛 (一土 // 廿 // 中大)

---

- Cangjie Ordering Rules:

- Two elements top; one element bottom
- 勢 (土戈 // 大尸) - double unit
  - 熱 (土戈 // 火) - double unit
  - 藝 (廿 // 土戈 // 戈) - triple unit
- 警 (廿大 // 卜一口) - double unit
- 贊 (竹山 // 月山金)
- 聚 (尸水 // 人人人)
- 鴛 (弓山 // 竹日火) - double unit
- 聲 (土水 // 尸十) - double unit character
- 蟹 (弓手 // 中一戈) - double unit
- 巽 (口山 // 廿金) - double unit
  - 撰 (手 // 口山 // 金) - triple unit
- 操 (手 // 口口 // 木) - triple unit
  - 藻 (廿 // 水 // 口木) - triple unit
- 堅 (尸水 // 土) - double unit
  - 賢 (尸水 // 月山金)A - double unit
- 艦 (竹卜 // 尸戈 // 廿) - triple unit
- 雚 (廿 // 口口 // 土)
  - 觀 (廿土 // 月山山)
  - 歡 (廿土 // 弓人)
- 諧 (卜口 // 心心 // 日) - triple unit
- 歷 (一 // 竹木 // 一) - triple unit
- 寶 (十 // 一山 // 金) - triple unit
- 然 (月大 // 火) - double unit
- 關 (日弓 // 女戈 // 廿) - triple unit
  - 潔 (水 // 手竹 // 火)
- 盜 (水人 // 月廿) - double unit
- 操 (手 // 口 // 口木) - triple unit
  - 躁 (口一 // 口 // 口木)
- 桑 (水 // 水水 // 木) - triple unit, mulberry
  - 顙 (水木 // 一月金)
- 輩 (中卜 // 十田十) - double unit
  - 罪 (田中 // 中一 // 卜) - triple unit
- 導 (卜山 // 木戈)

- One element top; two elements bottom:
- 露 (一月 // 口一 // 口) - triple unit
- 覆 (一田 // 竹人 // 水) - triple unit
- 發 (弓人 // 弓 // 竹水) - triple unit
- 爺 (金大 // 尸十 // 中) - triple unit
- 舛 (弓戈 // 手)
- 粦 (火木 // 弓戈 // 手)
  - 鄰 (火手 // 弓中) - double unit
  - 憐 (心 // 火木 // 手 )
- 厨 (一 // 一廿 // 戈)
- 蘇 (廿 // 弓火 // 木)

- 殺 (大金 // 竹弓 // 水) - triple unit
- 疑 (心大 // 弓戈人) - double unit
  - 凝 (戈一 // 心大 // 人) - triple unit
- 務 (弓竹 // 人大 // 尸) - triple unit
- 贛 (卜十 // 竹水 // 金) - triple unit
- 徽 (竹人 // 山火 // 大)
- 薛 (廿 // 竹口 // 十) - triple unit
- 能 (戈月 // 心心)
  - 態 (戈心 // 心) - double unit
- 龍 (卜月 // 卜尸心)
- 假 (人 // 口尸 // 水)
  - 霞 (一月 // 口尸 // 水) - triple unit
- 覵 (日月 // 月山山)
- 綴 (女火 // 水水 // 水) - triple unit
- 解 (弓月 // 尸竹 // 手) - triple unit

- 類 (火大 // 一月金)
- 甄 (一土 // 一女弓)
- 穎 (心木 // 一月金) - double unit
- 数 (火女 // 人大)
- 迎 (卜 // 竹女 // 中) - triple unit

## Notable Character Codes

### 金, 人, Human Leg

#### Human Leg, Animal Leg

- 儿 (中山)
- 先 (竹土竹山) - before
  - 贊 (竹山 // 月山金)
- 旡 (一女大山) - waitress

- 兄 (口竹山) - teenager
- 兌/兑 (金 // 口竹山) - devil
  - 說 (卜口 // 金 // 口山)
  - 说 (戈女 // 金 // 口山)
  - 脫 (月 // 金 // 口山)

- 只 (口金) - only
  - 识 (戈女 // 口 // 金)
- 总 (金口 // 心)

- 元 (一一山) vs. 克 (十口竹山)
- 見 (月山竹山) - see
  - 現 (一土 // 月山山) - double unit character, take F2+L of the second unit
  - 親 (卜木 // 月山山) - double unit
- 见 (月 // 竹山)
  - 现 (一土 // 月 // 竹山)
  - 视 (戈火 // 月竹山)
  - 舰 (竹卜 // 月竹山)
- 冠 (月 // 一山 // 戈)

- 竟 (卜廿 // 日竹山) - mirror
  - 境 (土 // 卜廿 // 山) - triple unit

- 貝 (月山金)
  - 贝 (月人)
- 頁 (一月山金)
- 页 (一月人)
  - 颗 (田木 // 一月人)
- 貢 (一月山金)

- 金 is 儿 in compound
  - 儿 (中山)
- 穴 (十金) - animal leg, hole
  - 空 (十金 // 一) double unit character
  - 穿 (十金 // 一女竹) - double unit

- 深 (水 // 月金 // 木) - miniskirt
- 探 (手 // 月金 // 木)

- 西 (一金田) - west
  - 覀 (一中中田)
  - 曲 (廿田)
- 酉 (一金田一) - whisky
  - 酌 (一田 // 心戈)
  - 酒 (水 // 一金田)
  - 配 (一田 // 尸山)
- 酋 (廿金田一) - chieftain
  - 尊 (廿田 // 木戈) - double unit
  - 鄭 (廿大 // 弓中)

- 高 (卜 // 口 // 月口) - triple unit, high
  - 豪 (卜 // 口 // 月人) - triple unit

- 夭 (竹大)
  - 笑 (竹 // 竹大)
- 喬 (竹大 // 口 // 月口) - triple unit
  - 乔 (竹大 // 中中) - angel
  - 橋 (木 // 竹大 // 月)
  - 嬌 (女 // 竹大月)
  - 娇 (女 // 竹大中)
- 春 (手大 // 日) - spring, bonsai
- 眷 (火手 // 月山)

- 介 (人中中) - introduce
  - 界 (田人中中) - world
- 亦 (卜中弓金)
- 弗 (中中弓)
  - 佛 (人 // 中中弓)

- 賈 (一田 // 月山金) - double unit, merchant
  - 贾 (一田月人)
- 要 (一田 // 女)
- 覆 (一田 // 竹人 // 水) - triple unit
- 甄 (一土 // 一女弓)

- 四 (田金) - number four
- 陸 (弓中 // 土金 // 土)

- 尤 (戈大山) - Frankenpooch
  - 就 (卜火 // 戈大山)
  - 弋 (戈心) - arrow
- 无 (一大山) - nothing
- 旡 (一女大山) - Waitress
  - 既 (日戈 // 一女山)
  - 贊 (竹山 // 月山金)
- 冘 (中月山) - sinking, sink
  - 枕 (木 // 中月山)
- 元 (一一山)

#### Gold, metal 金

- 分 (金 // 尸竹) - divide, parts
  - 份 (人 // 金尸竹)

- 公 (金戈) - public

- 丫 (金中)

- 交 (卜金大) - mingle

- 曾 (金 // 田 // 日) - increase
  - 僧 (人 // 金田日) - monk
  - 贈 (月金 // 金田日)
- 曹 (廿田 // 日)
- 賈 (一田 // 月山金) - double unit

- 谷 (金人口) - valley
  - 卻 (金口 // 尸中)
  - 俗 (人 // 金人口)
  - 豁 (十口 // 金人口)
- 容 (十 // 金人口)
  - 蓉 (廿 // 十 // 金口)
- 沿 (水金口) - gully

- 釵 (金 // 水戈)

#### Meeting

- meeting
- 會 (人 // 一 // 田日) - triple-unit (only take the F+L of the 3rd unit)
  - 繪 (女火 // 人 // 一日)
  - 会 (人 // 一 // 一戈) - triple-unit
- 侖 (人一月廿) - academic conference
  - 輪 (十十 // 人一 // 月) - triple unit
  - 崙 (山 // 人 // 一月) - triple unit

- 俞 (人 // 一 // 月弓) - triple unit, butcher's meeting
  - 喻 (口 // 人 // 一弓) - triple unit
  - 輸 (十十 // 人 // 一弓) - triple-unit character
- 前 (廿 // 月 // 中弓) - triple unit

- 合 (人 // 一 // 口) - fit
  - 哈 (口 // 人 // 一口)
  - 答 (竹 // 人 // 一口)
  - 給 (女火 // 人 // 一口)

---

- 倉 (人 // 戈 // 日口) - triple unit

- 今 (人戈弓) - clock
  - 念 (人 // 戈 // 弓心) - triple unit
  - 仱 (人 // 人 // 戈弓)
- 令 (人 // 戈 // 弓戈) - triple unit, order
  - 伶 (人 // 人 // 戈戈) - triple unit
  - 怜 (心 // 人 // 戈戈)

- 陰 (弓中 // 人 // 戈戈) - triple unit

#### Human, Person 人

- 仁 (人一一)

- 從 (竹人 // 人人 // 人) - triple unit, accompany
- 緃 (女火 // 人人 // 人)
  - 縱 (女火 // 竹人 // 人) - vertical
- 然 (月大 // 火) - double unit, flesh

- 德 (竹人 // 十田 // 心) - triple unit

- 豕 (一尸竹人) - sow, pig, F3+L
  - 豪 (卜 // 口 // 月人) - triple unit
- 家 (十 // 一尸人) - double unit, house
  - 嫁 (女 // 十 // 一人) - triple unit
- 勿 (心竹竹)
- 昜 (日 // 一尸竹)

- 象 (弓日心人) - one unit character, take F3+L, elephant
  - 像 (人 // 弓日人)
- 飞 (弓人) - fly
- 衤 (戈弓中人)
- 长 (心人)

- 豸 (月尸竹竹) - skunk
  - 貌 (月竹 // 竹日山)

- 久 (弓人)
  - 𥹰 (火女 // 弓人)

- 入 (人竹) vs. 八 (竹人) - enter, eight
- 人 (人)
- 夫 (手人)

- 之 (戈弓人) - chiii

- 免 (弓日竹山) - rabbit
  - 晚 (日 // 弓日山) - double unit
  - 鬼 (竹山戈) - ghost
- 兔 (弓日竹戈)
- 兎 (竹中日戈)
- 象 (弓日心人) - one unit character, take F3+L
  - 像 (人 // 弓日人)

- 兆 (中一山人)

- 飞 (弓人)
  - 飛 (弓人 // 竹廿人) - fly

- 以 (女戈人) - plow
  - 似 (人 // 女戈人)

- 眾 (田中 // 人人人) - mass
- 聚 (尸水 // 人人人)

- 祭 (月人 // 一一火) - offer sacrifice
  - 際 (弓中 // 月人 // 火) - triple unit
  - 示 (一一火)
- 豋 (月人 // 一口廿)

- 癶 (弓戈 // 卜人) - A tipi or tepee
- 登 (弓人 // 一 // 口廿) - ascend
  - 燈 (火 // 弓人 // 廿) - triple unit
- 豆 (一口廿)

- 餐 (卜水 // 人 // 戈女)

- 尞 (大金 // 日火) - pup tent
  - 遼 (卜 // 大金 // 火)

- 卒 (卜 // 人人 // 十)
- 雜 (卜木 // 人土)

- 冰 (戈一 // 水)
- 冷 (戈一 // 人 // 戈戈) - triple unit
- 弱 (弓一 // 弓戈一)

- 个 (人中)

### Corpse, flag 尸

- 己 (尸山) - snake
  - 巳 (口山) - sign of the snake
  - 記 (卜口 // 尸山)
  - 妃 (女 // 尸山)
  - 改 (尸山 // 人大)
  - 起 (土人 // 尸山)
- 已 (尸山)
- 卩 (尸中)
- 弓

- 㔾 (尸山) - fingerprint
  - 苑 (廿 // 弓戈 // 山) - triple unit
  - 鴛 (弓山 // 竹日火) - double unit
  - 範 (竹 // 十十 // 山)
- 仓 (人 // 尸山)
  - 沧 (水 // 人 // 尸山) - triple unit
- 倉 (人 // 戈 // 日口) - triple unit, godown
  - 創 (人口 // 中 // 弓) - triple unit
- 厄 (一 // 尸山) - unlucky
  - 呃 (口 // 一 // 尸山)

- 巽 (口山 // 廿金) - double unit
  - 撰 (手 // 口山 // 金) - triple unit
  - 選 (卜 // 口山 // 金)

- broom
- 求 (戈十水) - request
- 彔 (女弓一水)
  - 录 (弓一一水)
  - 碌 (一口 // 女弓水)
  - 錄 (金 // 女弓水)
- 祿 (戈火 // 女弓水)
  - 禄 (戈火 // 弓一水)
- 綠 (女火 // 女弓水)
  - 緑 (女火 // 弓一水)
  - 绿 (女一 // 弓一水)
- 灵 (尸一 // 火)
- 屬 (尸 // 水 // 田戈) - triple unit

- 灵 (尸一 // 火)
  - 𠴌 (口 // 尸一火)
- 雪 (一月 // 尸一)
- 歸 (竹一 // 尸一 // 月) - triple unit
- 当 (火尸一)
- 彗 (手十 // 尸一)
  - 慧 (手十 // 尸一 // 心)
- 虐 (卜心 // 尸一)

- 尹 (尸大) - mop
- 君 (尸大口) - old boy
  - 群 (尸口 // 廿手)
  - 羊 (廿手)

- 長 (尸一女) - long, mane
  - 长 (心人)
- 張 (弓 // 尸一女)
  - 张 (弓心人)
- 辰 (一一一女) - sign of the dragon

- 刀 (尸竹) - blade, sword, dagger
  - 昭 (日 // 尸竹口)
  - 留 (竹竹 // 田) - letter openner
  - 分 (金 // 尸竹)
  - 寡 (十 // 一金 // 竹) - triple unit
- 召 (尸竹 // 口) - seduce
  - 紹 (女火 // 尸竹 // 口) - triple unit
  - 招 (手 // 尸竹 // 口)
  - 超 (土人 // 尸竹 // 口) - triple unit

- 刅 (尸竹金)
- 刃 (尸竹戈)
  - 忍 (尸戈 // 心)
  - 粱 (水戈 // 火木)
  - 認 (卜口 // 尸戈 // 心) - triple unit

- 創 (人口 // 中弓) - saber
- 前 (廿 // 月 // 中弓) - triple unit

- 力 (大尸) - power
  - 甥 (竹一 // 田大尸)
  - 癆 (大 // 火火 // 尸)
  - 边 (卜 // 大尸)
  - 为 (戈大尸戈)
- 加 (大尸 // 口)
  - 賀 (大口 // 月山金) - double unit
  - 咖 (口 // 大尸 // 口)
- 劦 (大尸 // 大尸 // 尸) - triple unit
  - 協 (十 // 大尸 // 尸)
- 別/别 (口尸 // 中 // 弓) - separate
  - 另 (口大尸)
- 刀 (尸竹) vs. 刃 (尸竹戈)

- 男 (田 // 大尸)
  - 舅 (竹難 // 田 // 大尸)

- 司 (尸 // 一 // 口) - company, clothes hanger
  - 嗣 (口月 // 尸 // 一口)
  - 詞 (卜口 // 尸 // 一口)
- 幻 (女戈 // 尸)

- 成 (戈竹尸) - turn into
- 方 (卜竹尸) - compass, direction
  - 房 (竹尸 // 卜竹尸)
  - 芳 (廿 // 卜竹尸)
  - 邊 (卜 // 竹山 // 尸)
- 万 (一尸)
- 分 (金 // 尸竹) - divide
- 別/别 (口尸 // 中 // 弓) - separate
  - 另 (口大尸)

- 旁 (卜月 // 卜竹尸) - side
  - 螃 (中戈 // 卜月 // 尸) - triple unit
  - 傍 (人 // 卜月 // 尸)

- banner
- 於 (卜尸 // 人 // 卜)
- 族 (卜尸 // 人 // 人大)
- 旅 (卜尸 // 人 // 竹女) - group of people
- 游 (水 // 卜尸 // 木) - triple unit
  - 斿 (卜尸 // 人 // 弓木) - triple unit

- 馬 (尸手尸火) - horse
  - 碼 (一口 // 尸手火)
  - 驚 (廿大 // 尸手火) - double unit
- 马 (弓女尸一)
  - 吗 (口 // 弓女一)
  - 妈 (女 // 弓女一)
- 与 (卜尸一)

- 鳥 (竹日卜火) - single unit, bird
  - 島 (竹日卜山)
  - 鴻 (水 // 一 // 竹火) - triple unit
  - 鳳 (竹弓 // 一日火) - phoenix
- 鸟 (心卜尸一)
  - 鹃 (口月 // 心卜一)
  - 鸭 (田中 // 心卜一)
- 烏 (竹口卜火) - crow
  - 乌 (心女尸一)

- 號 (口尸 // 卜心山)

- 耳 (尸十) - ear
  - 聲 (土水 // 尸十) - double unit character
  - 聰 (尸十 // 竹田 // 心) - triple unit
  - 取 (尸十 // 水)
- 最 (日 // 尸十 // 水) - triple unit
- 敢 (一十 // 人大) or (弓十 // 人大) - daring

- 乍 (人尸) - saw
  - 作 (人 // 人尸)
  - 怎 (人尸 // 心)
- 面 (一田尸中 or 一田卜中) - mask, noodle
- 斤 (竹一中) - axe, drag

- 那 (尸手 // 弓中) - Alcatraz
  - 哪 (口 // 尸手 // 中) - triple unit

- 阝 (弓中)
- 卩 (尸中)
- 爺 (金大 // 尸十 // 中) - triple unit
  - 爷 (金大 // 尸中)
- 刀 (尸竹)

- 卬 (竹女 // 尸中) - double unit, stamp collection
  - 迎 (卜 // 竹女 // 中) - triple unit
  - 仰 (人 // 竹女 // 中) - triple unit

- 卯 (竹竹 // 尸中) - Sign of The Hare
  - 卵 (竹竹 // 尸中戈)
  - 柳 (木 // 竹竹 // 中)

- 秀 (竹木 // 弓竹尸) - excel
  - 透 (卜 // 竹木 // 尸) - triple unit
- 乃 (弓竹尸) - fist
  - 為 (戈大弓火)
  - 刀 (尸竹)
- 力 (大尸)

- 族 (卜尸 // 人人大) - banner

- 扁 (竹尸 // 月廿) - comic book
  - 偏 (人 // 竹尸 // 月) - triple unit
  - 遍 (卜 // 竹尸 // 月)
  - 篇 (竹 // 竹尸 // 月)
- 侖 (人一月廿) - academic conference
  - 輪 (十十 // 人一 // 月) - triple unit
  - 轮 (大手 // 人心)
  - 崙 (山 // 人 // 一月) - triple unit
  - 淪 (水 // 人一月)

- 叚 (口尸 // 尸水) - nerd
  - 假 (人 // 口尸 // 水)
  - 霞 (一月 // 口尸 // 水) - triple unit

- 户 (戈尸)
- 居 (尸 // 十口) - reside
  - 剧 (尸口 // 中 // 弓)

- 局 (尸尸口) - bureau

- 匾 (尸竹尸月) - cardboard box
- 匵 (尸土田金) - single unit
- 區 (尸口口口) - ward, region
  - 奩 (大 // 尸口口)
  - 区 (尸大)
- 凶 (山大) - villain, shovel

- 臣 (尸中尸中) - retainer, slave
  - 臥 (尸中 // 人)
  - 巨 (尸尸) - gigantic
  - 片 (中中一中) - F3+L
  - 熙 (尸山 // 火) - double unit
- 臤 (尸中 // 水)
  - 堅 (尸水 // 土) - double unit
  - 賢 (尸水 // 月山金) - double unit
- 臨 (尸中 // 人 // 口口) - triple unit
  - 临 (中 // 中 // 人日) - triple unit

- 監 (尸戈 // 月廿) - double unit, oversee, hidden camera
  - 艦 (竹卜 // 尸戈 // 廿) - triple unit
  - 藍 (廿 // 尸戈 // 廿) - triple unit
  - 覧 (尸一 // 月山山) - double unit
  - 览 (中戈 // 月竹山) - double unit
  - 覽 (尸田 // 月山山) - double unit
- 监 (中戈 // 月廿) - double unit
  - 蓝 (廿 // 中戈 // 廿) - triple unit
  - 览 (中戈 // 月竹山) - double unit

- 亡 (卜女)

- 眉 (日竹 // 月山) - divided flag, eyebrow
  - 湄 (水 // 日竹 // 山)
- 屓 (尸 // 月山金)
- 色 (弓日山) - color
- 聲 (土水 // 尸十) - double unit character
  - 声 (土 // 日竹)
- 倉 (人 // 戈 // 日口) - triple unit, godown
  - 創 (人口 // 中 // 弓) - triple unit

- 尼 (尸心) - nun
  - 怩 (心 // 尸心)
  - 呢 (口 // 尸心)

### 弓, 戈, 心, 竹

#### Bow 弓

- 歹 (一弓戈) - malicious, bones
  - 殃 (一弓 // 中月大)
  - 列 (一弓 // 中 // 弓)
  - 殘 (一弓 // 戈戈)
  - 殖 (一弓 // 十月一)

- chop seal
- 通 (卜 // 弓戈月) - double unit, take F2+L of second unit
  - 甬 (弓戈月手) - pogo stick
- 爷 (金大 // 尸中)
  - 节 (廿 // 尸中)
- 疑 (心大 // 弓戈人) - double unit
  - 凝 (戈一 // 心大 // 人) - triple unit

- 色 (弓日山) - color

- 子 (弓木); - children
  - 好 (女 // 弓木)
  - 字 (十 // 弓木)
  - 李 (木 // 弓木)
- 了 (弓弓) - completed
  - 辽 (卜 // 弓弓)
- 享 (卜口弓木) - receive
  - 惇 (心 // 卜口木)

- 承 (弓弓手人) - F3+L

- 乃 (弓竹尸) - fist
- 及 (弓竹水) - outstretched hands
  - 仍 (人 // 弓竹尸)
  - 級 (女火 // 弓竹水)

- 永 (戈弓水)
- 又 (弓大)

- 為 (戈大弓火)

- 廴 (弓弓大)
  - 建 (弓大 // 中手)

- 几 (竹弓) - wind, weather vane
  - 風 (竹弓 // 竹中戈) - double unit
  - 没 (水 // 竹弓 // 水)
  - 処 (竹水 // 竹弓)
- 凡 (竹弓戈) - mediocre
- 鳳 (竹弓 // 一日火) - phoenix
  - 鳥 (竹日卜火)
  - 凤 (竹弓 // 水)

- 飞 (弓人)
  - 飛 (弓人 // 竹廿人) - fly
- 气 (人一弓) - air
  - 氣 (人弓 // 火木) - double unit
  - 気 (人弓 // 大) - double unit

- 亢 (卜 // 竹弓) - whirlwind
  - 抗 (手 // 卜竹弓)
  - 骯 (月月 // 卜竹弓)

- 丁 (一弓) - spike, nail
  - 頂 (一弓 // 一月金)
- 𡨸 (十弓十弓木)
- 何 (人 一弓口) - what
  - 荷 (廿 // 人 // 一口)

- 彳 (竹人)
- 亍 (一一弓) - nail, spike
- 行 (竹人 // 一一弓) - column, boulevard, queue
  - 衡 (竹人 // 弓大 // 弓) - triple unit
  - 蘅 (廿 // 竹人 // 弓) - triple unit
  - 得 (竹人 // 日一 // 戈) - triple unit

- 可 (一弓 // 口) - cannn
  - 啊 (口 // 弓中 // 口) - triple unit
  - 河 (水 // 一弓口)
- 奇 (大 // 一弓口) - strange
  - 寄 (十 // 大 // 一口) - triple unit - triple unit

- 才 (木竹) - genie
  - 团 (田 // 木竹)
  - 閉 (日弓 // 木竹)
  - 財 (月金 // 木竹)
  - 在 (大中 // 土)
  - 存 (大中 // 弓木)
- 之 (戈弓人) - chiii
- 勿 (心竹竹)

- 久 (弓人)
  - 𥹰 (火女 // 弓人)
- 夕 (弓戈)

- 阝 (弓中) - pinnacle
  - 阜 (竹口 // 十) - double unit
- 都 (十日 // 弓中)
  - 者 (十大日) - doll

- 卩 (尸中) - postage stamp
  - 卻 (金口 // 尸中)
  - 命 (人 // 一 // 口中) - triple unit
  - 服 (月 // 尸中水) - apparel
- 即 (日戈 // 尸中) - instant
  - 節 (竹 // 日戈 // 中) - triple unit

- 节 (廿 // 尸中)
  - 爷 (金大 // 尸中)

- 㔾 (尸山) - fingerprint
  - 苑 (廿 // 弓戈 // 山) - triple unit
  - 鴛 (弓山 // 竹日火) - double unit

- 殳 (竹弓水) - missile
  - 段 (竹十 // 竹弓水)
  - 投 (手 // 竹弓水)
  - 殺 (大金 // 竹弓 // 水) - triple unit
- 發 (弓人 // 弓 // 竹水) - triple unit
  - 癶 (弓戈卜人)
- 沒 (水 // 弓水)

- 乾 (十十 // 人弓) - fish hook

- 予 (弓戈弓弓) - beforehand
- 矛 (弓戈弓竹) - halberd, F3+L
  - 務 (弓竹 // 人大 // 尸) - triple unit
- 疑 (心大 // 弓戈人) - double unit

- 角 (弓月土) - bound up
- 換/换 (手 // 弓月大)

- 魚 (弓田火) - fish
  - 魯 (弓田 // 火 // 日) - triple unit
  - 蘇 (廿 // 弓火 // 木)
- 鱼 (弓田一)
  - 渔 (水 // 弓田一)

- 姨 (女 // 大弓)
- 姊 (女 // 中難竹) - elder sister
- 第 (廿 // 弓中竹)

- 欠 (弓人) - lack
  - 歡 (廿土 // 弓人)
  - 次 (戈一 // 弓人)
  - 欢 (水 // 弓人)
- 欸 (戈大 // 弓人)

- 尔 (弓火) - thou
  - 称 (竹木 // 弓火)
  - 你 (人 // 弓火)

- 勿 (心竹竹)
- 句 (心口) - phrase

- 卩 (尸中) - postage stamp

- 买 (弓 // 卜大) - buy
- 卖 (十弓卜大) - sell

#### Halberd, fiesta (戈)

- 厶 (女戈) - elbow
- 公 (金戈)
- 強 (弓 // 戈 // 中戈) - triple unit
  - 虫 (中一戈)
- 轉 (十十 // 十戈 // 戈) - triple unit
- 虽 (口中一戈) - although
  - 雖 (口戈 // 人土)

- Wall
- 至 (一戈土) - climax
  - 到 (一土 //  中 //  弓) Triple unit character
  - 倒 (人 // 一土 // 弓)
  - 侄 (人 // 一戈土)
  - 臺 (土 // 口 // 月土) - pedestal
- 云 (一一戈) - rising cloud
- 能 (戈月 // 心心)

- 惠 (十戈 // 心) - favor, double unit
  - 穗 (竹木 // 十戈 // 心) - triple unit

- 專 (十戈 // 木戈) - double unit, specialty
  - 團 (田 // 十戈 // 戈) - triple unit
  - 傳 (人 // 十戈 // 戈) - triple unit
- 专 (手弓戈) - corncob pipe

- 㐬 (卜戈竹中山)
- 流 (水 // 卜戈山) - infant Moses
- 充 (卜戈竹山) - allot
  - 統 (女火 // 卜戈 // 山)

- 台 (戈口)
  - 治 (水 // 戈口)

- 或 (戈口一) - orrr
  - 國 (田 // 戈口一) - country
  - 国 (田 // 一土戈)
  - 惑 (戈一 // 心) - double unit, (nghi) hoặc
  - 域 (土 // 戈口一)
- 彧 (戈大口一)

- 戊 (戈竹) - parade
- 戉 (戈女)
  - 越 (土人 // 戈女) - surpass, việt
- 成 (戈竹尸) - turn into
  - 城 (土 // 戈竹尸) - castle
- 戚 (戈竹 // 卜一火) - double unit
  - 蹙 (戈火 // 口卜人) - double unit
  - 尗 (卜一火)

- 戌 (戈竹一) - march
- 咸 (戈竹 // 一口) - hàm
  - 感 (戈口 // 心) - double unit, cảm
- 鹹 (卜田 // 戈竹 // 口) - triple unit
  - 鹵 (卜田戈戈) - Rock salt

- 幾 (女戈 // 竹戈)
- 𢦏 (十戈) - Thanksgiving
  - 裁 (十戈 // 卜竹女)
  - 戴 (十戈 // 田廿金) - double unit
  - 載 (十戈 // 十田十)

- 㦰 (人人戈)
- 韱 (人戈 // 中尸一) - double unit, anime convention
  - 籤 (竹 // 人戈 // 一) - triple unit
  - 韭 (中一一一 or 中尸一一)

- 戠 (卜日戈) - kazoo
  - 識 (卜口 // 卜戈 // 日) - triple unit
  - 織 (女火 // 卜戈 // 日) - triple unit

- 戈
  - 伐 (人戈) - fell

- 弋 (戈心) - arrow
  - 代 (人 // 戈心) - substitute, age, era
  - 膩 (月 // 戈心 // 金) - triple unit
  - 式 (戈心 // 一)
- 武 (一心 // 卜中一) - double unit, quiver, warrior
  - 賦 (月金 // 一心 // 一) - triple unit
- 民 (口女心)
- 尤 (戈大山) - Frankenpooch

- 淺 (水 // 戈戈) - float
  - 浅 (水 // 戈十)
  - 殘 (一弓 // 戈戈)

- 求 (戈十水)

- 我 (竹手戈)
  - 哦 (口 // 竹手戈)

- 九 (大弓) - nine
- 丸 (大弓戈) - bottle of pills
  - 紈 (女火 // 大弓戈)
- 力 (大尸)

- 坴 (土金 // 土) - Mushroom Kingdom
  - 陸 (弓中 // 土金 // 土)
- 埶 (土土 // 大弓戈) - double unit, Mario
- 熱 (土戈 // 火) - double unit
  - 藝 (廿 // 土戈 // 戈) - triple unit
  - 云 (一一戈) - rising cloud
- 勢 (土戈 // 大尸) - double unit

- 夌 (土金 // 竹水) - toad
  - 菱 (廿 // 土金 // 水)

#### Heart 心

- 戈 - halberd
  - 伐 (人戈) - fell

- 弋 (戈心) - arrow
- 代 (人 // 戈心) - substitute, age, era
  - 黛 (人心 // 田土 // 火) - triple unit
    - 黑 (田土 // 火) - double unit
  - 膩 (月 // 戈心 // 金) - triple unit
- 武 (一心 // 卜中一) - double unit, quiver
  - 賦 (月金 // 一心 // 一) - triple unit

- 氏 (竹女心) - family name
  - 民 (口女心)
  - 婚 (女 // 竹心 // 日) - triple unit
- 氐 (竹心 // 一) - double unit, Business card
  - 低 (人 // 竹心 // 一) - triple unit

- 世 (心廿)
- 也 (心木)
- 长 (心人)
- 五 (一木一)
- 九 (大弓)
- 丸 (大弓戈)
- 帶 (大心 // 月中月)

- 乇 (竹心)
- 毛 (竹手山) - fur

- 化 (人心) - transform
  - 花 (廿 // 人心) - flower
- 混 (水 // 日 // 心心)

- 旨 (心日) - delicious
  - 脂 (月 // 心日)

- 能 (戈月 // 心心) - double unit, ability
  - 態 (戈心 // 心) - double unit
- 龍 (卜月 // 卜尸心) - dragon

- 勿 (心竹竹) - knot
  - 物 (竹手 // 心竹竹)
- 易 (日 // 心竹竹)
- 昜 (日 // 一尸竹) - piggy bank
  - 陽 (弓中 // 日 // 一竹) - triple unit
  - 場 (土 // 日 // 一竹) - triple unit
  - 楊 (木 // 日 // 一竹)
- 杨 (木 // 弓尸竹)

- 勾 (心戈)
  - 构 (木 // 心戈)

- 勺 (心戈) - ladle
  - 的 (竹日 // 心戈)
  - 酌 (一田 // 心戈)
- 砲 (一口 // 心口山) - double unit
- 荀 (廿心日)
- 句 (心口) - phrase
  - 夠 (弓弓 // 心口)
  - 可 (一弓口)

- 敬 (廿口 // 人大) - double unit, awe, revere
  - 驚 (廿大 // 尸手火) - double unit
  - 警 (廿大 // 卜一口) - double unit
- 露 (一月 // 口一 // 口) - triple unit
- 殺 (大金 // 竹弓 // 水) - triple unit

- 包 (心口山) - wrap
  - 跑 (口一 // 心口山)
  - 抱 (手 // 心口山)
- 負 (弓月山金) - defeat
  - 负 (弓月人)

- 钅 (人一心) - metal, gold
  - 钟 (人心 // 中)
  - 铁 (人心 // 竹手人)
  - 错 (人心 // 廿 // 日)

- 想 (木山 // 心) - double unit

- 必 (心竹) - invariably

- 比 (心心) - compare
  - 批 (手 // 心心)
  - 諧 (卜口 // 心心 // 日) - triple unit
  - 崑 (山 // 日 // 心心)

#### Bamboo 竹

- 管 (竹十口口)
- 話 (卜口 // 竹十口)

- 彡 (竹竹竹) - shape
- 形 (一廿 // 竹竹竹) - double unit
  - 开 (一廿)
- 影 (日火 // 竹竹竹)
  - 景 (日 // 卜 // 口火)
- 修 (人 // 中 // 人竹)
- 㐱 (人 // 竹竹竹)
  - 參 (戈 // 戈戈 // 竹)
  - 珍 (一土 // 人 // 竹竹)
  - 寥 (十 // 尸一 // 竹) - triple unit
  - 蓼 (廿 // 尸一 // 竹)
- 髟 (尸戈 // 竹竹竹) - Hairstyle
  - 髮 (尸竹 // 戈大大)

- 綴 (女火 // 水水 // 水)

- 宓 (十心竹)

- 眉 (日竹 // 月山)

### Sun 日

- 明 (日月)

- 早 (日十)
  - 贛 (卜十 // 竹水 // 金) - triple unit
- 卓 (卜日十)

### Moon 月

- 卜 = hat
- 月= claw, crown
  - 冠 (月 // 一山 // 戈)
  - 冤 (月 // 弓日戈)
- 十 = house (roof)

- 爭 (月 // 尸木) - claw, vulture
- 稱 (竹木 // 月 // 土月)

- 受 (月月水) - Birdhouse
- 愛 (月月 // 心 // 水) - triple unit, love
- 隱 (弓中 // 月一心) - double unit, take F2+L of the second unit
  - 隐 (弓中 // 弓尸心)
- 学 (火月 // 弓木)

- 豸 (月尸竹竹) - skunk
  - 貌 (月竹 // 竹日山)
- 稱 (竹木 // 月 // 土月)

- 奚 (月 // 女戈 // 大) - eagle
  - 徯 (竹人 // 月 // 女大)

- 爰 (月一大水) - migrating ducks
  - 緩 (女火 // 月一水)

- 亂 (月月 // 山)

- 骨 (月月月) - skeleton
  - 體 (月月 // 廿田 // 廿) - triple unit, body
  - 髓 (月月 // 卜 // 大月)
  - 骯 (月月 // 卜竹弓)
  - 髒 (月月 // 廿 // 一廿)
- 咼 (月月口) - jawbone
  - 過 (卜 // 月月口)
  - 媧 (女 // 月月口)
  - 禍 (戈火 // 月月口)

- 雨 (一中月卜) - 卜 is two dấu phết, rain
  - 露 (一月 // 口一 // 口) - triple unit
  - 雪 (一月 // 尸一) - snow
- 雲 (一月 // 一一戈)
  - 云 (一一戈) - rising cloud

- 兩 (一中月人) - F3+L
- 市 (卜中月)

- 巾 (中月) - towel
  - 市 (卜中月) - market
  - 佩 (人 // 竹弓 // 月) - triple unit
- 帝 (卜月中月) - emperor
  - 啼 (口卜月月)
- 制 (竹月 // 中 // 弓) - system
  - 製 (竹弓 // 卜竹女) - double unit

- 帛 (竹日中月) - brocade
  - 錦 (金 // 竹日月)

- 帶 (大心 // 月中月) - sash
  - 带 (廿十 // 月中月)
  - 帯 (廿中 // 月 // 中月)
- 世 (心廿)

- 內 (人月) - inside
- 肉 (人月人) - meat
- 丙 (一人月)
  - 病 (大 // 一人月)
- 兩 (一中月人) - both
  - 両 (一山月)
  - 两 (一人人月)
  - 滿 (水 // 廿中月)
  - 満 (水 // 廿山月)

- 革 (廿中十)
- 黃 (廿一田金)
- 嘆 (口 // 廿中人)
- 寅 (十 // 一中金)
- 禺 (田中月戈) - F3+L
- 电 (中田山)
- 更 (一中田大)
  - 便 (人 // 一中大)

- 离 (卜 // 山大 // 月) - oddjob, detach
  - 璃 (一土 // 卜 // 山月) - triple unit

- 且 (月一) - book shelf
  - 姐 (女 // 月一)
  - 組 (女火 // 月一)
  - 祖 (戈火 // 月一)
- 目 (月山) - eye
  - 眼 (月山 // 日女)
  - 首 (廿竹月山)
- 自 (竹月山) - oneself, nose, nostrils
  - 息 (竹山 // 心)
- 相 (木 // 月山)
  - 湘 (水 // 木 // 月山)
  - 想 (木山 // 心) - double unit
- 具 (月一一金) - tool

- 直 (十月一一) - straightaway, straight
  - 值 (人 // 十月一)
  - 置 (田中 // 十月一)
  - 植 (木 // 十月一)
  - 殖 (一弓 // 十月一)

- 真 (十月一金) - true, F3 + L
  - 顛 (十金 // 一月金)
- 眞 (心 // 月山 // 金) - triple unit
  - 滇 (水 // 十月金)

- add one (一)
- 且 (月一)
- 直 (十月一一)
- 具 (月一一金)
- 真 (十月一金) - true, F3 + L
- 其 (廿一一金)
- 甘 (廿一)
- 共 (廿金)

- 敻 (弓月 // 月山 // 水 or 弓月月山大) - triple unit
  - 瓊 (一土 // 弓月 // 水) - triple unit

- 冉 (土月)
  - 再 (一土月) - again
  - 稱 (竹木 // 月 // 土月)
- 冓 (廿廿土月) - funnel
  - 講 (卜口 // 廿廿月)

- 角 (弓月土) - angle
  - 嘴 (口 // 卜心 // 月) - triple unit
- 解 (弓月 // 尸竹 // 手) - triple unit
  - 蟹 (弓手 // 中一戈) - double unit

- 寒 (十 // 廿金 // 卜) - triple unit, celery
- 襄 (卜 // 口口 // 女) - Grass skirt
  - 孃 (女 // 卜 // 口女) - double unit
  - 讓 (卜口 // 卜 // 口女)

- 補 (中 // 戈十月)

- 禺 (田中月戈) - cricket, F3+L
  - 偶 (人 // 田中月) - double unit
- 萬 (廿 // 田中月) - ten thousand
  - 万 (一尸)
- 耦 (手木 // 田中月)
  - 藕 (廿 // 手木 // 月)
  - 耒 (手木)

- 虫 (中一戈) - insect
  - 蟠 (中戈 // 竹木 // 田) - triple unit
  - 風 (竹弓 // 竹中戈) - double unit, wind
  - 䖝 (竹中一戈)
  - 蟹 (弓手 // 中一戈) - double unit
  - 閩 (日弓 // 中一戈)
- 蜀 (田中 // 心 // 中戈) - triple unit, Shu Han
  - 獨 (大竹 // 田中 // 戈)
- 屬 (尸 // 水 // 田戈) - triple unit
  - 属 (尸 // 竹中月)

- 然 (月大 // 火) - double unit, flesh
  - 犬 (戈大)
- 厭 (一 // 日月 // 大) - triple unit
- 將 (女一 // 月 // 木戈) - triple unit
- 望 (卜月 // 竹土) - double unit; F+L // F2+L
- 遙 (卜 // 月 // 人山) - triple unit
- 祭 (月人 // 一一火)
- 登 (弓人 // 一口廿)

- 夕 (弓戈) - evening
  - 久 (弓人)
- 外 (弓戈卜)
- 多 (弓戈弓戈)
  - 夠 (弓弓 // 心口)
  - 句 (心口) - phrase, sentence
- 名 (弓戈口) - name
- 夢 (廿 // 田中 // 弓) - triple unit, dream

- 歹 (一弓戈) - malicious, bones
  - 殯 (一弓 // 十 // 一金)
- 死 (一弓 // 心) - double unit, death
  - 葬 (廿 // 一心 // 廿) - interment
  - 髒 (月月 // 廿 // 一廿)

- 同 (月一口) - same
  - 洞 (水 // 月一口) - cave

- 周 (月 // 土口) - circumference
  - 調 (卜口 // 月 // 土口)
  - 调 (戈女 // 月 // 土口)
  - 週 (卜 // 月 // 土口)

- 典 (廿月金) - code, canon, điến
- 曲 (廿田) - bent, bend, khúc
  - 體 (月月 // 廿田 // 廿) - triple unit, body

- 運 (卜 // 月 // 十十) - triple unit

- 鬲 (一 // 口 // 月中) - triple unit, camera
  - 融 (一月 // 中一戈)
- 南 (十月廿十) - south
- 羊 (廿手)
- 辛 (卜廿十)
- 幸 (土廿十)
- 壴 (土 // 口廿) - drum
  - 喜 (土 // 口廿 // 口) - single unit

- 豆 (一口廿) - table, bean
  - 厨 (一 // 一廿 // 戈)

- 册 (月月一)

### Middle 中

- 事 (十中中弓) - rake
- 捷 (手 // 十中人)
- 肅 (中難)
- 庚 (戈 // 中人)

- 爭 (月 // 尸木)

- 聿 (中手) - brush
  - 筆 (竹 // 中手)
  - 茟 (廿 // 中手)
  - 建 (弓大 // 中手)
- 書 (中土 // 日) - book
- 畫 (中土 // 田一) - a drawing
  - 劃 (中一 // 中 // 弓)
- 书 (戈木尸)
- 盡 (中一 // 火 // 月廿) - triple unit

- 灵 (尸一 // 火)
- 雪 (一月 // 尸一)
- 歸 (竹一 // 尸一月)
- 录 (弓一一水)
- 当 (火尸一)
- 尹 (尸大) - mop
- 君 (尸大口) - old boy

- 止 (卜中一) - footprint
  - 歸 (竹一 // 尸一月)
  - 此 (卜一 // 心) - double unit
- 正 (一卜中一) - correct
  - 政 (一一 // 人大)
- 延 (弓大 // 竹卜一) - prolong
- 誕 (卜口 // 弓大 // 一) - triple unit, nativity
  - 诞 (戈女 // 弓大 // 女) - triple unit

- 匕 (山竹) - spoon
  - 此 (卜一 // 心) - double unit
  - 些 (卜心 // 一 // 一) - triple unit character; take F-L of the first unit

- 是 (日 // 一卜人) - double unit
- 𤴓 (一卜人)

- 風 (竹弓 // 竹中戈) - double unit, wind
  - 虫 (中一戈)

- 中 = cloak (衤)
- 衤 (戈弓中人) - cloak
- 被 (中 // 木竹水)
- 褔 (中一口田)

- 示 (一一火)
  - 宗 (十 // 一一火)
- 礻 (戈弓火) - altar
  - 視 (戈火 // 月山山)
  - 祐 (戈火 // 大口)
  - 禪 (戈火 // 口口 // 十) - triple unit
  - 祖 (戈火 // 月一)
- 衣 (卜竹女)

- 复 (人 // 日 // 竹水) - Double back
  - 複 (中 // 人 // 日水) - triple unit
  - 腹 (月 // 人 // 日水) - triple unit
  - 覆 (一田 // 竹人 // 水) - triple unit

- 申 (中田中) - monkey
  - 神 (戈火 // 中田中)
- 由 (中田) - sprout
- 甲 (田中)
  - 押 (手 // 田中)

- 央 (中月大) - center
- 英 (廿 // 中月大) - england
  - 瑛 (一土 // 廿中大) - double unit
- 春 (手大 // 日)
- 夫 (手人)
- 电 (中田山)

- 婁 (中田中女) - sniper
  - 樓 (木 // 中田女)
  - 數 (中女 // 人大)
- 事 (十中中弓)
- 實 (十 // 田十 // 金) - triple unit

- 亞 (一中中一), 亜 (一中中一) - asia, F3+L
  - 惡 (一一 // 心), 悪 (一一 // 心)

- 工 (一中一) - craft

- 貴 (中一 // 月山金) - double unit

- 寅 (十 // 一中金) - sign of the tiger, F3+L
  - 演 (水 // 十 // 一金) - triple unit
- 电 (中田山)

- 亦 (卜中弓金)
- 州 (戈中戈中)
- 兆 (中一山人)
- 介 (人中中)
- 北 (中一 // 心)

- 而 (一月中中) - comb
  - 懦 (心 // 一月 // 月)

- 漢 (水 // 廿中人) - scarecrow
- 嘆 (口 // 廿中人)
- 難 (廿人 // 人土) - double unit, difficult
- 革 (廿中十) - leather

- 川 (中中中) - stream, flood
  - 圳 (土 // 中中中)
- 州 (戈中戈中) - châu, F3+L
  - 洲 (水 // 戈中中) - double unit
- 巢 (女女 // 田木) - stream
  - 緇 (女火 // 女女 // 田)
- 片 (中中一中) - F3+L

- 儿 (中山)

- 师 (中 // 中 // 一月) - triple unit

### Fire 火

- 爾 (一火月大) - thou
  - 彌 (弓 // 一火月)
  - 瀰 (水 // 弓 // 一月) - triple unit

- 京 (卜 // 口火) - capital
  - 谅 (戈女 // 卜 // 口火)

- 示 (一一火)

- 小 (弓金) - small
  - 孙 (弓木 // 火)
- 少 (火竹) - few
  - 省 (火竹 // 月山)
  - 妙 (女 // 火竹)
- 賓 (十 // 一竹 // 金) - triple unit, vippp
  - 檳 (木 // 十 // 一金)
  - 殯 (一弓 // 十 // 一金)

- 步 (卜中一竹) - walk, F3+L
  - 频 (卜竹 // 一月人)

- 尔 (弓火) - thou
  - 称 (竹木 // 弓火)
  - 你 (人 // 弓火)

- 不 (一火) - negative
  - 还 (卜 // 一火)
- 礻 (戈弓火)
  - 禪 (戈火 // 口口 // 十) - triple unit

- 求 (戈十水)
- 彔 (女弓一水)
  - 祿 (戈火 // 女弓水)
- 灵 (尸一 // 火)

- 光 (火一山) - ray
  - 辉 (火山 // 月 // 大手) - triple unit

- 礻 (戈弓火)
  - 不 (一火)
- 視 (戈火 // 月山山)
- 衣 (卜竹女)
- 神 (戈火 // 中田中)

- 卷 (火手尸山)

- 米 (火木) - rice
  - 氣 (人弓 // 火木) - double unit
  - 数 (火女 // 人大)
  - 未 (十木)
- 類 (火大 // 一月金)
  - 类 (火木 // 大)

- 釆 (竹火木)
  - 番 (竹木 // 田) - double unit, dice, dropping
  - 蟠 (中戈 // 竹木 // 田) - triple unit
- 乎 (竹火木)
- 平 (一火十) - water lily
  - 評 (卜口 // 一火十)

- 半 (火手) - half
  - 判 (火手 // 中 // 弓)
- 羊 (廿手)

- 果 (田木) - fruit, confectionary
  - 顆 (田木 // 一月金)
- 巢 (女女 // 田木) - stream

- owl
- 巣 (火 // 田木)
- 單 (口口田十) - simple
  - 禪 (戈火 // 口口十)
- 単 (火田十)
  - 单 (金田十)
- 兴 (火一金)

- 学 (火月 // 弓木) - School House
  - 學 (竹月 // 弓木) - double unit, study
  - 栄 (火月 // 木)
  - 举 (火金 // 手)
- 瑩 (火火 // 月 // 一戈) - triple unit, HOT HOUSE
  - 榮 (火火 // 月 // 木) - triple unit
  - 栄 (火月 // 木)
- 劳 (廿 // 月 // 大尸) - GREEN HOUSE
- 寬 (十 // 廿 // 月戈) - triple unit
- 受 (月月水)

- 龸 (火月) - outhouse
  - 當 (火月 // 口田)
  - 棠 (火月 // 口木)
  - 堂 (火月 // 口土)
  - 常 (火月 // 口中月)

- 叔 (卜火 // 水) - double unit, uncle
  - 寂 (十 // 卜火 // 水)
- 上 (卜一) - above
- 卡 (卜一卜)
- 尗 (卜一火)

- 求 (戈十水)

- 尚 (火月口)

- 肖 (火月) - candle
  - 銷 (金 // 火月)
  - 鎖 (金 // 火月金)
  - 鋇 (金 // 月山金)
  - 趙 (土人 // 火月)

- 炎 (火火) - inflamation
  - 談 (卜口 // 火火)

- 点 (卜口 // 火)

### 廿, 一

#### Twenty 廿 (nhập)

- bộ thảo (grass)
- 懂 (心 // 廿 // 竹土)
  - 重 (竹十田土)
- 蓮 (廿 // 卜 // 十十) - triple unit

- 頁 (一月山金) - page, head
  - 頭 (一廿 // 一月金)
  - 豆 (一口廿)
  - 寡 (十 // 一金 // 竹) - triple unit
  - 憂 (一月 // 心 // 竹水) - melancholy
- 页 (一月人)
  - 颗 (田木 // 一月人)

- 首 (廿竹月山)
  - 道 (卜 // 廿竹山)
- 百 (一日) - hundred

- 貝 (月山金) - shell
- 贝 (月人)
  - 质 (竹 // 十月人)
- 貢 (一月山金)
- 員 (口 // 月山金) - employee
  - 员 (口 // 月人)
  - 圓 (田 // 口 // 月金) - triple unit

- 業 (廿金廿木) - profession
  - 业 (難廿金)
- 並 (廿廿金) - row
  - 普 (廿金 // 日)
- 對 (廿土 // 木戈)

- 关 (廿大)
- 前 (廿 // 月 // 中弓) - triple unit
- 美 (廿土大) - beautiful
- 善 (廿土廿口) - virtuous
- 羊 (廿手)

- 乎 (竹火木)

- 皿 (月廿) - dish
  - 豔 (山廿 // 土戈 // 廿) - triple unit
  - 寧 (十 // 心 // 月弓) - triple unit
  - 盡 (中一 // 火 // 月廿) - triple unit
  - 盜 (水人 // 月廿)
- 盃 (一火 // 月廿)
  - 𢝙 (心 // 一火 // 廿)
- 血 (竹月廿) - blood
- 蓋 (廿 // 土戈 // 廿) - triple unit

- 無 (人廿火) - nothing
  - 蕪 (廿 // 人廿火)
  - 撫 (手 // 人廿火)

- 共 (廿金) - strung together
- 異 (田 // 廿金)
  - 暴 (日 // 廿金 // 水)
- 庶 (戈 // 廿火) - commoner

- 其 (廿一一金) - rook, bushel basket
  - 期 (廿金 // 月) - double unit characters, only take F+L of the first unit
  - 基 (廿金 // 土) - double unit character, only take F+L of the first unit
- 甘 (廿一) - wicker basket
  - 某 (廿一木)
  - 甜 (竹口 // 廿一)
- 且 (月一)

- 甚 (廿一一女)
  - 勘 (廿女 // 大尸)
  - 堪 (土 // 廿一女)

- 㒼 (廿中月人)
- 昔 (廿日) - salad
  - 惜 (心 // 廿日)
- 革 (廿中十) - leather
- 黃 (廿一田金) - yellow
- 嘆 (口 // 廿中人)

- 展 (尸廿女)

- 开 (一廿) - two hands, open
- 形 (一廿 // 竹竹竹) - double unit
  - 彡 (竹竹竹)
- 刑 (一廿 // 中弓)
  - 型 (一弓 // 土) - double unit
- 并 (廿廿) - puzzle
  - 拼 (手 // 廿廿)

- 井 (廿廿) - well
  - 进 (卜 // 廿廿)

- 升 (竹廿) - measuring box
- 昇 (日 // 竹廿)

- 世 (心廿)

- 立 (卜廿) - stand up, vase
  - 接 (手 // 卜廿 // 女)
  - 親 (卜木 // 月山山) - double unit
  - 泣 (水 // 卜廿)
  - 啦 (口 // 手 // 卜廿)
- 帝 (卜月中月)

- 啇 (卜金月口) - F3+L, antique vase
  - 嫡 (女 // 卜金月) - double unit
- 商 (卜金月口) - make a deal

- 音 (卜廿 // 日) - sound
  - 暗 (日 // 卜廿 // 日)
  - 韻 (卜日 // 口月金) - double unit
  - 意 (卜廿 // 日 // 心)
- 咅 (卜廿口) - muzzle
  - 部 (卜口 // 弓中) - double unit

- 辛 (卜廿十) - spicy
  - 𨐌 (卜廿手)
  - 新 (卜木 // 竹一中)
  - 宰 (十 // 卜廿十)
  - 親 (卜木 // 月山山) - double unit
  - 辣 (卜十 // 木中)
- 辟 (尸口 // 卜廿十) - repudiate
  - 避 (卜 // 尸口 // 十)
  - 薛 (廿 // 竹口 // 十) - triple unit
  - 癖 (大 // 尸口 // 十)
  - 僻 (人 // 尸口 // 十)
- 幸 (土廿十) - happy, happiness
  - 達 (卜 // 土廿手)
- 睪 (田中 // 土廿十) - testicle
  - 譯 (卜口 // 田中 // 十)
  - 睾 (竹田 // 土廿十)
- 南 (十月廿十) - south

- 虛 (卜心 // 廿一) - cactus
- 關 (日弓 // 女戈 // 廿) - triple unit

- 黃 (廿一田金 or 廿一中金) - yellow
  - 廣 (戈 // 廿一金) - double unit wide

- 義 (廿土 // 竹手戈) - righteous, double unit
  - 义 (戈大)
  - 議 (卜口 // 廿土 // 戈) - triple unit
  - 儀 (人 // 廿土 // 戈)
  - 我 (竹手戈)
- 父 (金大) - father, dad
  - 交 (卜 // 金大) - mingle
  - 爹 (金大 // 弓戈 // 弓) - triple unit
- 爺 (金大 // 尸十 // 中) - triple unit
  - 爷 (金大 // 尸中)

- 壴 (土 // 口廿) - drum
- 喜 (土 // 口廿 // 口) - triple unit, joyful
  - 嘻 (口 // 土 // 口口) - triple unit
  - 禧 (戈火 // 土 // 口口)
- 臺 (土 // 口 // 月土) - pedestal
- 善 (廿土廿口)

- 華 (廿 // 一廿十) - splendor
  - 譁 (卜口 // 廿一十)
- 垂 (竹十廿一) - silage
  - 睡 (月山 // 竹十一)
- 乗 (竹木廿) - ride
  - 乘 (竹木 // 中心)

- 莫 (廿 // 日大) - graveyard
  - 模 (木 // 廿日大)
  - 寞 (十 // 廿日大)
  - 漠 (水 // 廿 // 日大)

#### One `一`

- 馬 (尸手尸火) - horse
  - 碼 (一口 // 尸手火)
  - 驚 (廿大 // 尸手火) - double unit

- 丁 (一弓)
- 𡨸 (十弓十弓木)

- 場 (土 // 日 // 一竹) - triple unit
- 勿 (心竹竹)

- 紅 (女火 // 一) - double unit
- 隱 (弓中 // 月一心) - double unit, take F2+L of the second unit

- 巫 (一人人)

- 元 (一一山) vs. 克 (十口竹山) - origin
  - 頑 (一山 // 一月金)
  - 远 (卜 // 一一山)
  - 院 (弓中 // 十 // 一山) - triple unit
- 光 (火一山) - ray
- 爿 (女中一)

- 丂 (一女尸)
- 号 (口一女尸)

- 頁 (一月山金)
  - 頭 (一廿 // 一月金)
  - 豆 (一口廿)
- 首 (廿竹月山) - neck, head
  - 道 (卜 // 廿竹山)
- 百 (一日) - hundred
- 白 (竹日) - white, dove

- 道 (卜 // 廿竹山) - double unit, road
  - 導 (卜山 // 木戈)
  - 导 (口山 // 木戈)

- 羽 (尸一 // 尸戈一) - double unit; F+L // F2+L, feather, wing
  - 寥 (十 // 尸一 // 竹) - triple unit
  - 蓼 (廿 // 尸一 // 竹)

- 冰 (戈一 // 水) - iceee
  - 次 (戈一 // 弓人)
  - 准 (戈一 // 人土)
  - 率 (卜 // 戈人 // 十)
- 冷 (戈一 // 人 // 戈戈) - triple unit, cold
- 凝 (戈一 // 心大 // 人) - triple unit
  - 疑 (心大 // 弓戈人) - double unit
- 弱 (弓一 // 弓戈一)

- 韭 (中一一一 or 中尸一一) - leek, F3+L
- 韱 (人戈 // 中一一 or 人戈 // 中尸一) - double unit, anime convention
  - 籤 (竹 // 人戈 // 一) - triple unit
- 齏 (卜難 // 中一一)

- 兆 (中一 // 山人)
- 非 (中一 // 尸卜) - double unit, jail
  - 輩 (中卜 // 十田十) - double unit
  - 悲 (中卜 // 心)
  - 罪 (田中 // 中一 // 卜) - triple unit
  - 排 (手 // 中一 // 卜)
- 北 (中一 // 心) - north
- 化 (人心)

- 工 (一中一) - craft
  - 江 (水 // 一)

### 水, 大, 木

#### Water 水

- 冬 (竹水卜) & 科 (竹木 // 卜十) - winter
- 永 (戈弓水) - eternity
  - 樣 (木 // 廿土 // 水) three unit; take last radical of the 3rd unit
- 求 (戈十水)
- 決 (水 // 木大); 治 (水 // 戈口)

- 冰 (戈一 // 水)
- 冷 (戈一 // 人 // 戈戈) - triple unit
- 弱 (弓一 // 弓戈一)

- 去 (土戈) - gone
  - 法 (水 // 土戈)

- crotch, again
- 又 (弓大) - 水 = 又 vs. 大 = dấu x (メ)
  - 支 (十水) - branch
  - 臤 (尸中 // 水)
  - 欢 (水 // 弓人)
- 夂 (竹水) - walking legs
  - 夏 (一山 // 竹水) - double unit
  - 备 (竹水 // 田)
- 処 (竹水 // 竹弓)
  - 處 (卜心 // 竹水 // 弓)
- 各 (竹水口) - to each his own
  - 落 (廿 // 水 // 竹口) - triple unit
  - 格 (木 // 竹水 // 口)

- 叉 (水戈)
  - 釵 (金 // 水戈)

- 雙 (人土水)
  - 双 (難水水)

- 桑 (水 // 水水 // 木) - triple unit, mulberry
  - 顙 (水木 // 一月金)
- 躁 (口一 // 口 // 口木)

- 攵 (人大) - taskmaster
  - 故 (十口 // 人大)
  - 做 (人 // 十口 // 大)
- 矢 (人大)

- 文 (卜大) - literature
  - 这 (卜 // 卜大)

- 求 (戈十水) - request
- 彔 (女弓一水)
  - 录 (弓一一水)
  - 碌 (一口 // 女弓水)

- 暴 (日 // 廿金 // 水)

- 礻 (戈弓火)
- 米 (火木) - rice

#### Big, large 大

- 老 (十大心) - old man
- 者 (十大日) - doll
  - 都 (十日 // 弓中)
  - 著 (廿 // 十大日)
- 考 (十大卜尸)
- 孝 (十大弓木) - filial piety
  - 教 (十木 // 人大)

- 更 (一中田大) - grow late
  - 便 (人 // 一中大) - convenience

- 為 (戈大弓火) - viii
- 爲 (月竹弓火)
- 为 (戈大尸戈)

- 左 (大一) - left, by one's side
- 右 (大口) => by one's side là 大; right
  - 若 (廿 // 大口)
- 有 (大月) - possess
- 隨 (弓中 // 卜 // 大月) - triple unit
  - 髓 (月月 // 卜 // 大月)
- 在 (大中土) - genie
- 帶 (大心 // 月中月)

- 友 (大水) - friend
- 犮 (戈大大)
  - 拔 (手 // 戈大大)
  - 髮 (尸竹 // 戈大大) - hair
- 发 (戈女大水)
  - 泼 (水 // 戈女水) - double unit
- 反 (竹水) - anti-, against
  - 版 (中中 // 竹水)

- 九 (大弓)
- 丸 (大弓戈)
- 埶 (土土 // 大弓戈) - double unit, Mario
- 熱 (土戈 // 火) - double unit
  - 藝 (廿 // 土戈 // 戈) - triple unit
  - 云 (一一戈) - rising cloud

- 又 (弓大) - again, crotch
- 故 (十口 // 人大)

- 文 (卜大) - literature
  - 这 (卜 // 卜大)

- 矢 (人大) - dart
  - 欸 (戈大 // 弓人)
  - 族 (卜尸 // 人人大) - banner
- 知 (人大 // 口)
  - 椥 (木 // 人大 // 口)
- 攵 (人大) - taskmaster

- 天 (一大) - heaven
- 吴 (口 // 一大) - Ngô
- 吳 (口 // 女弓大)
  - 誤 (卜口 // 口女大)
- 夭 (竹大)
  - 笑 (竹 // 竹大)
- 喬 (竹大 // 口 // 月口) - triple unit
  - 乔 (竹大 // 中中)
  - 橋 (木 // 竹大 // 月)

- 力 (大尸)

- 犬 (戈大) - chihuahua
  - 厭 (一日月大)
- 太 (大戈)

- 換/换 (手 // 弓月大)

- 犭 (大尸竹) or (大弓竹) - pack of wild dogs
  - 狼 (大竹 // 戈日女)
  - 狂 (大竹 // 一土)
  - 獲 (大竹 // 廿人水)
  - 獄 (大竹 // 卜口 // 大) - triple unit

- 史 (中大) - history
- 吏 (十中大)
  - 使 (人 // 十中大)
- 更 (一中田大) - grow late
  - 便 (人 // 一中大) - convenience
- 丈 (十大) - trượnggg

- 央 (中月大)
- 春 (手大 // 日)
- 夫 (手人)

#### Drag, Cliff, Cave, Sick Bed

- 竹 = Drag
- 𠂆 (竹竹)
- 反 (竹水) - anti, against
- 篇 (竹 // 竹尸 // 月)
- 爬 (竹人 // 日山)
- 氏 (竹女心)

- 戶 (竹尸) - door
  - 房 (竹尸 // 卜竹尸)
  - 扁 (竹尸 // 月廿)
  - 遍 (卜 // 竹尸 // 月)
  - 户 (戈尸)

- 爪 (竹中人) - clawww, vulture
- 瓜 (竹女戈人) - melon
  - 孤 (弓木 // 竹女人)

- 旅 (卜尸 // 人 // 竹女) - triple unit, group of people
- 脈 (月 // 竹 // 竹女) - zombie

- 斤 (竹一中) - axe, drag
  - 所 (竹尸 // 竹一中)
  - 芹 (廿 // 竹一中)
  - 新 (卜木 // 竹一中)
  - 質 (竹中 // 月山金) - double unit
  - 芹 (廿 // 竹一中)
- 折 (手 // 竹一中)
  - 逝 (卜 // 手 // 竹中) - triple unit
- 后 (竹一口) - queen

- 一 = 厂 (Cliff)
- 厭 (一 // 日月 // 大) - triple unit
  - 壓 (一大土) - double unit
- 辰 (一一一女)
- 歷 (一 // 竹木 // 一) - triple unit
- 压 (一 // 土戈)

- 原 (一 // 竹日火) - meadow
  - 願 (一火 // 一月金)

- 詹 (弓金 // 卜一口) - verbose
  - 膽 (月 // 弓金 // 口) - triple unit

- 右 (大口) => by one's side là 大
- 石 (一口) - stone
  - 破 (一口 // 木竹水)
  - 硯 (一口 // 月山山)

- 戈 = cave
- 广 (卜竹) - cave
- 度 (戈 // 廿 // 水) - cavern
- 廣 (戈 // 廿一金) - double unit, wide
  - 黃 (廿一田金) - yellow
- 廁 (戈 // 月金 // 弓) - triple unit
  - 則 (月金 // 中 // 弓) - triple unit
- 府 (戈 // 人 // 木戈)
- 廟 (戈 // 十十 // 月) - triple unit
- 膺 (戈 // 人土 // 月)
- 床 (戈 // 木)

- sick bed: 病 (大 // 一人月)
  - 疼 (大 // 竹水卜)
  - 痰 (大 // 火火)
  - 疾 (大 // 人大)
  - 癆 (大 // 火火 // 尸)
- 丙 (一人月)

#### Tree, wood 木

- 寸 (木戈) - glue
  - 村 (木 // 木戈) - village
  - 壽 (土弓一戈) - longevity
  - 守 (十 // 木戈)
  - 專 (十戈 // 木戈) - double unit, specialty
- 寺 (土木戈) - Buddhist temple, pagoda
  - 待 (竹人 // 土 // 木戈) - triple unit
  - 時 (日 // 土 // 木戈) - triple unit
  - 诗 (戈女 // 土木戈) - poem

- 才 (木竹)
- 牙 (一女木竹) - tusk
  - 呀 (口 // 一女竹)
  - 穿 (十金 // 一女竹) - double unit

- 㝵 (日一 // 木戈)
  - 得 (竹人 // 日一 // 戈) - triple unit

- 乎 (竹火木)
- 子 (弓木); - children
- 于 (一木) - potato
- 干 (一十) - dry
  - 幹 (十十 // 人 // 一十)
  - 岸 (山 // 一 // 一十) - triple unit
  - 軒 (十十 // 一十)
- 士 (十一) - samurai

- 千 (竹十) - thousand
- 重 (竹十田土)

- 寺 (土木戈) - Buddhist temple, pagoda
  - 特 (竹手 // 土 木戈)
  - 時 (日 // 土木戈)
  - 侍 (人 // 土木戈)

- 也 (心木) - scorpion
  - 地 (土 // 心木)
  - 她 (女 // 心木)
- 世 (心廿) - generation
- 弋 (戈心) - arrow
- 右 (大口) => by one's side là 大

- 五 (一木一) - five
  - 吾 (一一 // 口) - double unit
  - 悟 (心 // 一一 // 口) - triple unit
  - 語 (卜口 // 一一 // 口) - triple unit

- 皮 (木竹水) - pelt
  - 被 (中 // 木竹水)
  - 破 (一口 // 木竹水)

- ユ (重木) - key
  - 侯 (人弓一大) - marquis
- 韋 (木一 // 口手)
- 五 (一木一)

- 夬 (木大) - Guillotine
  - 決 (水 // 木大)
  - 快 (心 // 木大)
- 年 (人手)

- 书 (戈木尸)

- 來 (木人人) - come, arrive, laiii
  - 来 (木廿)
- 夾 (大人人) - scissors
  - 夹 (大廿)
  - 陝 (弓中 // 大人人)
- 爽 (大大大大)

- 東 (木田) - east
  - 陳 (弓中 // 木田 ) - trần
- 东 (大木)
- 柬 (木田火)
- 闌 (日弓 // 木田火) - orchid
  - 攔 (手 // 日弓 // 田) - triple unit
  - 蘭 (廿 // 日弓 // 田) - triple unit
- 車 (十田十)
- 母 (田卜戈)

- 束 (木中) - bundle
  - 速 (卜 // 木中)
  - 辣 (卜十 // 木中)
  - 嫩 (女 // 木中 // 大)

- 親 (卜木 // 月山山) - double unit
- 立 (卜廿)

- 乐 (竹女木)

- 禾 (竹木) - wheat
  - 香 (竹 // 木日)
  - 瀋 (水 // 十竹 // 田)
- 秋 (竹木 // 火)
  - 愁 (竹火 // 心) - double unit

- 呆 (口木)
  - 保 (人 // 口木)
- 杏 (木口)

- 本 (木一)

### Ten 十

安 is 十 not 戈

- 営 (火月 // 口竹口)
- 宫 (十口口)

- 言 (卜一一口) - worddd
  - 許 (卜口 // 人十)
  - 這 (卜 // 卜一口)
- 讠 (戈弓女)
  - 谁 (戈女 // 人土)
  - 请 (戈女 // 手一 // 月) - triple unit
  - 诗 (戈女 // 土木戈) - poem
  - 记 (戈女 // 尸山)
  - 认 (戈女 // 人)

- 補 (中 // 戈十月) - dog tag
  - 甫 (戈十月)
- 稱 (竹木 // 月土月)

- 華 (廿 // 一廿十) - splendor
  - 譁 (卜口 // 廿一十)

- 末 (木十) - extremity, mạt
  - 抹 (手 // 木十)
- 未 (十木) - not yet, vị
  - 妹 (女 // 十木)
  - 米 (火木)
- 朱 (竹十木) - vermilion, chu
  - 姝 (女 // 竹十木)
  - 珠 (一土 // 竹十木)
- 耒 (手木) - christmas tree
  - 耦 (手木 // 田中月)

- 夫 (手人)
- 失 (竹手人)

### Fortune/divine Telling 卜

- 通 (卜 // 弓戈月) - road
- 進 (卜 // 人土)
  - 隹 (人土)
- 边 (卜 // 大尸)
- 迤 (卜 // 人 // 心木) - triple unit

- 廴 (弓弓大) - stretch
  - 建 (弓大 // 中手)
- 廷 (弓大 // 竹土)
  - 庭 (戈 // 弓大 // 土) - triple unit
  - 挺 (手 // 弓大 // 土)
- 延 (弓大 // 竹卜一) - prolong
- 誕 (卜口 // 弓大 // 一) - triple unit, nativity
  - 诞 (戈女 // 弓大 // 女) - triple unit
- 这 (卜 // 卜大)
  - 透 (卜 // 竹木尸) - double unit

- 外 (弓戈卜)

- 雨 (一中月卜) - 卜 is two dấu phết, rain
  - 巾 (中月)
- 冰 (戈一 // 水) vs. 科 (竹木 // 卜十)
- 图 (田 // 竹水卜) - map
  - 図 (田卜大)
- 斗 (卜十) - big dipper
- 头 (卜大) - bust
- 於 (卜尸 // 人卜)

- 冬 (竹水卜) - winter
  - 終 (女火 // 竹水 // 卜)
  - 疼 (大 // 竹水卜)

- 实 (十卜大)
- 實 (十 // 田十 // 金) - triple unit

- 走 (土卜人) - run
  - 趣 (土人 // 尸十 // 水) - triple unit
  - 起 (土人 // 尸山)
  - 超 (土人 // 尸竹 // 口) - triple unit
  - 赴 (土人 // 卜)
- 足 (口卜人) - peg leg

- 𤴓 (一卜人) - mending
  - 是 (日 // 一卜人) - double unit
- 疋 (弓卜人) - zoo
  - 疑 (心大 // 弓戈人) - double unit, zoo
  - 凝 (戈一 // 心大 // 人) - triple unit
- 正 (一卜中一) - correct
- 從 (竹人 // 人人 // 人) - triple unit

- 下 (一卜) - below
- 卞 (卜卜)
- 上 (卜一) - above

- 母 (田卜戈) - mom
- 每 (人 // 田卜戈) - double unit, every
  - 毎 (人 // 田十)
  - 海 (水 // 人 // 田卜) - triple unit, sea
  - 嗨 (口 // 水 // 人卜)
  - 敏 (人卜 // 人大) - double unit

- 丹 (月卜) - cinnabar
- 舟 (竹月卜戈) - boat
  - 般 (竹卜 // 竹弓 // 水) - triple unit
  - 艦 (竹卜 // 尸戈 // 廿) - triple unit

- 足 (口卜人) - peg leg
  - ⻊ (口卜中一)
  - 跛 (口一 // 木竹水)

- 悼 (心 // 卜日十)
- 上 (卜一) - above

### Woman 女

- 艮 (日女) - silver
- 良 (戈日女) - halo
  - 浪 (水 // 戈日女)
  - 娘 (女 // 戈日女)
  - 朗 (戈戈 // 月)
- 即 (日戈 // 尸中)
  - 節 (竹 // 日戈 // 中) - triple unit

- 食 (人 // 戈 // 日女) - food
  - 館 (人戈 // 十口口)
  - 喰 (口 // 戈人女)
  - 飲 (人戈 // 弓人)
- 饮 (弓女 // 弓人)
  - 饭 (弓女 // 竹水)

- 衣 (卜竹女) - garment
  - 表 (手一女)
  - 依 (人 // 卜竹女)
- 良 (戈日女)
- 辰 (一一一女) - sign of the dragon
- 旅 (卜尸 // 人 // 竹女) - triple unit

- 長 (尸一女) - long, mane
  - 长 (心人)
- 張 (弓 // 尸一女)
  - 张 (弓 // 心人)

- 袁 (土 // 口竹女) - double unit, yuan
  - 園 (田 // 土 // 口女) - triple unit
  - 遠 (卜 // 土 // 口女) - triple unit
- 圍 (田 // 木一 // 手) - triple unit

- 以 (女戈人)
- 糹 = 糸 = 女戈火 - thread
  - 總 (女火 // 竹田 // 心) - triple unit character, only take F+L of the first unit 糹
  - 紅 (女火 // 一)
  - 細 (女火 // 田)
- 纯 (女一 // 心山)
- 綠 (女火 // 女弓水)
  - 緑 (女火 // 弓一水)
  - 绿 (女一 // 弓一水)

- 紫 (卜心 // 女戈火)
- 素 (手一 // 女戈火)
- 索 (十月 // 女戈火)

- 幺 (女戈) - cocoon
  - 幻 (女戈 // 尸)
  - 幼 (女戈 // 大尸)
  - 後 (竹人 // 女戈 // 水) - triple unit
  - 關 (日弓 // 女戈 // 廿) - triple unit

- 亦 (卜中弓金) - apple
  - 变 (卜金 // 水)
  - 赦 (土金 // 人大)
- 變 (女火 // 人大)
  - 𤅶 (水 // 女火 // 大) - nôm
- 樂 (女戈 // 木) - sparkler
  - 楽 (戈人 // 木)
  - 率 (卜 // 戈人 // 十)

- 玄 (卜女戈) - Mysterious, Gandalf
- 兹 (廿 // 女戈 // 戈) - double mysterious
  - 磁 (一口 // 廿 // 女戈) - magnet

- 亥 (卜 // 女竹 // 人) - acorn
  - 刻 (卜人 // 中 // 弓)
  - 核 (木 // 卜女人)
  - 孩 (弓木 // 卜 // 女人)

- 系 (竹女戈火) - lineage, DNA
  - 係 (人 // 竹女火) - double unit
  - 孫 (弓木 // 竹女火)
  - 徽 (竹人 // 山火 // 大)
- 縣 (月火 // 竹女火)
  - 县 (月一戈)

- 甄 (一土 // 一女弓) - double unit
  - 瓦 (一女弓戈) - tile

- 彔 (女弓一水)

- 叫 (口 // 女中) - cornucopia

- 灣 (水 // 女火 // 弓) - triple unit

- 兆 (中一山人) - turtle shell
- 爿 (女中一) - bunk bed
  - 將 (女一 // 月 // 木戈) - triple unit
  - 壯 (女一 // 土)
  - 妝 (女一 // 女)
  - 石 (一口)
  - 鼎 (月山 // 女一 // 中)
- 片 (中中一中) - one sided, F3+L
  - 版 (中中 // 竹水)
- 臣 (尸中尸中)
  - 巨 (尸尸)

- 丂 (一女尸) - snare
- 号 (口 // 一女尸)
  - 號 (口尸 // 卜心 // 山)
- 元 (一一山)

- 牙 (一女木竹)
- 旡 (一女大山)
- 马 (弓女尸一)

- 巠 (一 // 女女 // 一)
  - 經 (女火 // 一 // 女一) - triple unit
  - 輕 (十十 // 一 // 女一)
- 圣 (水土) - spool
  - 怪 (心 // 水土)

- 衷 (卜 // 中竹女) - double unit
- 衰 (卜 // 田一女) - double unit

- 戉 (戈女)

### Hand 手

- monocle, sunglasses
- 舛 (弓戈 // 手) - pole dancer, evening...sunglasses
- 粦 (火木 // 弓戈 // 手)
  - 鄰 (火手 // 弓中) - double unit
  - 憐 (心 // 火木 // 手 )

- 夅 (竹水 // 手) - walking leg...monocle
  - 降 (弓中 // 竹水 // 手) - triple unit
- 年 (人手) - year, monocle
- 韋 (木一 // 口手) - locket
  - 韦 (手尸)
  - 圍 (田 // 木一 // 手) - triple unit
  - 衛 (竹人 // 木手 // 弓)
  - 違 (卜 // 木一手)
- 園 (田 // 土 // 口女) - triple unit
- 旡 (一女大山)
- 牙 (一女木竹)

- 表 (手一女) - surface, express

- 我 (竹手戈)
- 義 (廿土 // 竹手戈)
  - 議 (卜口 // 廿土 // 戈) - triple unit

- 指 (手 // 心日) - double unit
- 投 (手 // 竹弓水)
- 打 (手 // 一弓)
- 浙 (水 // 手 // 竹中) - triple unit

- 毛 (竹手山) - fur
  - 毫 (卜口月山)
- 乇 (竹心) - lock of hair
  - 托 (手 // 竹心)

- 丰 (手十) - bountiful, bushes
  - 邦 (手十 // 弓中)
  - 豐 (山十 // 一 // 口廿) - triple unit
  - 潔 (水 // 手竹 // 火)
- 拜 (竹手 // 一手十)
  - 拝 (手 // 一手十)
- 害 (十 // 手一 // 口) - triple unit
  - 轄 (十十 // 十 // 手口) - triple unit
- 夆 (竹水 // 手十)
  - 峰 (山 // 竹水 // 十) - triple unit
  - 逢 (卜 // 竹水 // 十) - triple unit

- 寿 (手大木戈) - longevity
  - 壽 (土弓一戈) - longevity

- 春 (手大 // 日) - spring, bonsai
  - 秦 (手大 // 竹木)
- 奉 (手大 // 手)
  - 捧 (手 // 手大手)
- 龹 (火手) - quarter
  - 眷 (火手 // 月山)
  - 拳 (火手 // 手)

- 夫 (手人) - father, husband
  - 芙 (廿 // 手人)
- 失 (竹手人) - lose, lost
  - 铁 (人心 // 竹手人)
- 未 (十木)

- 羊 (廿手) - sheep
  - 達 (卜 // 土廿手)
  - 鮮 (弓火 // 廿手)
  - 样 (木 // 廿手)
- ⺶ (廿手) - wool
  - 着 (廿手 // 月山)
  - 差 (廿手 // 一)
- 養 (廿人 // 戈日女)
- 看 (竹手 // 月山)
- 美 (廿土大)

- 生 (竹手一) - life, cell
  - 性 (心 // 竹手一)
  - 隆 (弓中 // 竹水 // 一) - triple unit
  - 甥 (竹一 // 田大尸)
  - 姓 (女 // 竹手一)
- 青 (手一 // 月) - blue, telescope
  - 晴 (日 // 手一 // 月)
  - 清 (水 // 手一 // 月)
- 積 (竹木 // 手一 // 金) - triple unit
  - 情 (心 // 手一月)

- 用 (月手) - utilize
- 角 (弓月土)
- 甬 (弓戈月手) - pogo stick
  - 通 (卜 // 弓戈月) - double unit, take F2+L of second unit

- 車 (十田十) - carrr
  - 連 (卜 // 十田十)
  - 琏 (一土 // 卜 // 大手)
  - 璉 (一土 // 卜 // 十十)
- 车 (大手)
  - 转 (大手 // 手弓戈) - double unit
  - 辉 (火山 // 月 // 大手) - triple unit
  - 轮 (大手 // 人心)
- 东 (大木)

- 牛 (竹手) - cow
  - 特 (竹手 // 土 // 木戈) - triple unit
  - 牡 (竹手 // 土)
  - 件 (人 // 竹手)
  - 物 (竹手 // 心竹竹)
  - 牧 (竹手 // 人大)
  - 解 (弓月 // 尸竹 // 手) - triple unit
- 制 (竹月 // 中 // 弓) - system
  - 製 (竹弓 // 卜竹女) - double unit

- 午 (人十) - noon
- 缶 (人十山) - tin can
  - 淘 (水 // 心 // 人山)
- 寶 (十 // 一山 // 金) - triple unit
- 䍃 (月 // 人十山) - Condor
  - 遙 (卜 // 月 // 人山) - triple unit

- 告 (竹土 // 口) - revelation, declare
  - 造 (卜 // 竹土口)

- 擊 (十水 // 手) - double unit

### Mountain 山

- 範 (竹 // 十十 // 山) - triple-unit character

- 寶 (十 // 一山 // 金) - triple unit
- 擊 (十水 // 手) - double unit

- 崇 (山十一火)

- 輝 (火山 // 月十十)

- 画 (一山田) - a drawing
  - 畫 (中土田一) - a drawing

- 乙 (弓山) - fish hook

- 巳 (口山)
- 巴 (日山) - mosaic
  - 吧 (口 // 日山)
- 色 (弓日山) - color
  - 絕 (女火 // 尸竹 // 山) - triple unit

- shovel
- 凶 (山大) - villain
  - 兇 (山大竹山)
  - 区 (尸大)
- 函 (山弓水)
  - 涵 (水 // 山 // 弓水) - triple unit
- 画 (一山田) - a drawing

- 齒 (卜一 // 山 // 人人) - tooth
  - 齡 (卜山 // 人 // 戈戈)
- 齿 (卜一 // 山人)
  - 龄 (卜山 // 人 // 戈戈)

- 屯 (心山) - barracks
  - 纯 (女一 // 心山)
  - 純 (女火 // 心山)
  - 頓 (心山 // 一月金)
- 屰 (廿山) - mountain goat
  - 塑 (廿月 // 土)

- 岡 (月 // 廿山)
- 冈 (月大)
  - 刚 (月大 // 中 // 弓)
- 罔 (月 // 廿 // 卜女)
  - 網 (女火 // 月 // 廿女)
  - 网 (月 // 大大)

- 缶 (人十山)

### Rice Field 田

- 典 (廿月金) - code, canon
- 曲 (廿田) - bent
- 豐 (山十 // 一 // 口廿) - triple unit
  - 豔 (山廿 // 土戈 // 廿) - triple unit
  - 丰 (手十)
- 豊 (廿田 // 一 // 口廿) - triple unit, bountiful
  - 體 (月月 // 廿田 // 廿) - triple unit
  - 禮 (戈火 // 廿田 // 廿) - triple unit

- 皿 (月廿) - dish

- 回 (田口)
- 圖 (田口卜田) - map
  - 图 (田 // 竹水卜) - map
  - 図 (田卜大)

- 因 (田大) - cause
  - 恩 (田大 // 心)
- 囚 (田人)

- 實 (十 // 田十 // 金) - triple unit
- 婁 (中田中女)

- 西 (一金田)
  - 覀 (一中中田)
  - 曲 (廿田) - bent, bend
- 賈 (一田 // 月山金) - double unit, merchant
  - 贾 (一田 // 月人)
  - 價 (人 // 一田金)
- 要 (一田 // 女)
- 甄 (一土 // 一女弓)

- 曹 (廿田 // 日) - cadet
- 會 (人 // 一 // 田日) - triple-unit character (only take the F+L of the 3rd unit)
  - 倉 (人 // 戈 // 日口) - triple unit
- 曾 (金 // 田 // 日) - double unit

- 里 (田土) - computer
  - 理 (一土 // 田土)
- 量 (日 // 一 // 田土) triple-unit character but the second unit only has one radical

- 童 (卜廿 // 田土)
  - 鐘 (金 // 卜廿 // 土)

- 重 (竹十田土) - heavy
  - 懂 (心 // 廿 // 竹土)
  - 種 (竹木 // 竹十土)
- 千 (竹十)

- 黑 (田土 // 火) - double unit, black
  - 點 (田火 // 卜口) - double unit
  - 黔 (田火 // 人戈弓)
  - 黛 (人心 // 田土火)

- 東 (木田) - east
  - 陳 (弓中 木田 )
- 柬 (木田火)
  - 闌 (日弓 // 木田火) - orchid
  - 攔 (手 // 日弓 // 田) - triple unit

- 母 (田卜戈) - mom, breast
- 毋 (田十)
- 每 (人 // 田卜戈) - double unit, every
  - 毎 (人 // 田十)
  - 海 (水 // 人 // 田卜) - triple unit, sea
  - 敏 (人卜 // 人大) - double unit
  - 梅 (木 // 人 // 田卜)

- 貫 (田十 // 月山金) - breast
  - 慣 (心 // 田十 // 金) - triple unit
  - 實 (十 // 田十 // 金) - triple unit
- 婁 (中田中女)

- 覽 (尸田 // 月山山) - double unit
- 览 (中戈 // 月竹山)

- 罒 (田中中) - **net**, eye on its side
- 曼 (日 // 田中 // 水) - triple unit, mandala
  - 漫 (水 // 日 // 田水) - triple unit
  - 慢 (心 // 日 // 田水)

- 鬟 (尸竹 // 田中 // 女)
- 夢 (廿 // 田中 // 弓) - triple unit, dream
  - 梦 (木木 // 弓戈)
- 還 (卜 // 田中女)
- 澤 (水 // 田中 // 十) - triple unit, espionage
- 睪 (田中 // 土廿十) - double unit
- 眾 (田中 // 人人人)
- 環 (一土 // 田中 // 女)
- 置 (田中 // 十月一)
- 覽 (尸田 // 月山山) - double unit

- 德 (竹人 // 十田 // 心) - triple unit
- 徳 (竹人 // 十田 // 心)

- 褱 (卜 // 田中 // 女) - triple unit, Darth vader
  - 懷 (心 // 卜田女) - double unit, reminisce

- 畺 (一田一一) - F3+L
  - 疆 (弓土 // 一田一)

- 悤 (竹田 // 心) - Microchip vaccine 5G
  - 聰 (尸十 // 竹田 // 心) - triple unit

- 卑 (竹竹十) - lowly
  - 顰 (卜金 // 竹竹 // 十) - triple unit
  - 俾 (人 // 竹竹 // 十)

- 畐 (一口田) - wealthy
  - 福 (戈火 // 一 // 口田) - triple unit

### Mouth 口

- 営 (火月 // 口竹口)
  - 呂 (口竹口) - spine
- 宫 (十口口)
  - 宮 (十 // 口竹口)

- 𠂤 (竹口中口) - maestro
  - 追 (卜 // 竹口口)
  - 薛 (廿 // 竹口 // 十) - triple unit
- 阜 (竹口 // 十) - double unit
  - 埠 (土 // 竹口 // 十) - triple unit

- 官 (十 // 口中口) - bureaucrat
  - 館 (人戈 // 十 // 口口) - triple unit

- 品 (口口口) - goods, product
  - 操 (手 // 口 // 口木) - triple unit
  - 藻 (廿 // 水 // 口木) - triple unit
  - 躁 (口一 // 口 // 口木)
- 桑 (水 // 水水 // 木) - triple unit, mulberry
  - 顙 (水木 // 一月金)

- 古 (十口) - ancient
  - 苦 (廿 // 十口)
- 固 (田十口)
  - 個 (人 // 田 // 十口)
- 胡 (十口 // 月)
  - 湖 (水 // 十口 // 月)

- 舌 (竹十口) - tongue
  - 活 (水 // 竹十口)

### Ground, soil 土

- 主 (卜土) - whole
  - 住 (人 // 卜土)
  - 註 (卜口 // 卜土)

- 王 (一土) - king
  - 聖 (尸口 // 竹土) - double unit
  - 壬 (竹土) - porter
  - 望 (卜月 // 竹土) - double unit; F+L // F2+L

- 玉 (一土戈) - jade
- 宝 (十一土戈)
  - 寶 (十 // 一山 // 金) - triple unit
- 全 (人 // 一土) - whole

- 呈 (口 // 竹土)
  - 程 (竹木 // 口 // 竹土)

- 在 (大中土)
- 左 (大一) - left, by one's side

- 角 (弓月土)

- 街 (竹人 // 土土 // 弓) - boulevard

- 圭 (土土) - ivy
  - 桂 (木 // 土土)
  - 封 (土土 // 木戈)
  - 珪 (一土 // 土土)
  - 閨 (日弓 // 土土)

- 吉 (土口) - lucky
  - 結 (女火 // 土口)

- 土
- 士 (十一) - samurai
- 干 (一十) - dry

- 丑 (弓土) - sign of the cow
  - 忸 (心 // 弓土)

### Others, 難, and Exceptions

=== 難

- 臼 (竹難) - mortar
  - 舊 (廿 // 人土 // 難) - triple unit
  - 舅 (竹難 // 田 // 大尸)
- 兒 (竹難竹山) - single unit
  - 儿 (中山) - er, child
- 叟 (竹難中水)
  - 嫂 (女 // 竹難水)
- 舀 (月 // 竹難)
  - 滔 (水 // 月 // 竹難)

- 與 (竹難卜金) - single unit, F3+L
  - 与 (卜尸一)
  - 舉 (竹金 // 手)
  - 举 (火金 // 手)
- 寫 (十 // 竹難火)
  - 写 (月 // 卜尸一)
- 马 (弓女尸一)
- 鸟 (心卜尸一)
- 乌 (心女尸一)

- 興 (竹難月金)
- 學 (竹月 // 弓木) - double unit, study
  - 𦥯 (竹難大月)
  - 覺 (竹月 // 月山山) - double unit
- 鼠 (竹難女卜女)

- 夷 (大弓)
  - 姨 (女 // 大弓) - aunt
- 姊 (女 // 中難竹) - elder sister
- 弗 (中中弓) - dollar
  - 拂 (手 // 中中弓)
  - 佛 (人 // 中中弓) - Buddha
- 第 (廿 // 弓中竹)
- 弟 (金 // 弓中竹)
  - 涕 (水 // 金弓竹)

- 齊 (卜難) - CSGT
  - 齋 (卜難火) - purification
  - 濟 (水 // 卜難)
  - 齏 (卜難 // 中一一)
- 卍 (弓難)
- 兼 (廿難金)
- 黽 (口難山)

- 身 (竹難竹) - body
- 射 (竹竹 // 木戈)
  - 榭 (木 // 竹竹 // 戈)

- 龜 (弓難山) - tortoise
  - 龝 (竹木 // 弓難山) - autumn

- 鹿 (戈難心) - deer
  - 塵 (戈心 // 土) - double unit
- 廌 (戈難火)
- 慶 (戈難水)

- 鼎 (月山 // 女一 // 中)
- 淵 (水 // 中難中) - abyss
  - 渊 (水中火中)
- 肅 (中難)
  - 瀟 (水 // 廿中難)
  - 繡 (女火 // 中難)

=== Others & Exceptions

- 門 (日弓) - gate
  - 開 (日弓 // 一廿)
  - 簡 (竹 // 日弓 // 日)
  - 覵 (日月 // 月山山)
- 门 (中戈尸)
  - 阊 (中尸 // 日日)
  - 间 (中尸 // 日)
  - 们 (人 // 中戈尸)
  - 简 (竹 // 中尸 // 日) - triple unit
  - 闫 (中尸 // 一一一)
  - 闩 (中尸 // 一)
- 鬥 (中弓) - big dipper
  - 鬧 (中弓 // 卜中月)
- 閨 (日弓 // 土土)
  - 闺 (中尸 // 土土)

- 畿 (女戈 // 田) - double unit
- 幾 (女戈 // 竹戈) - abacus
- 機 (木 // 女戈 // 戈) - triple unit

- 鬼 (竹山戈) - ghost
  - 塊 (土 // 竹山戈)
  - 魔 (戈木 // 竹山戈) - double unit
- 么 (竹戈)
  - 麼 (戈木 // 女戈) - double unit

- 虎 (卜心 // 竹山) - tiger
  - 虍 (tiger radical) (卜心)
  - 擄 (手 // 卜心 // 尸)
  - 處 (卜心 // 竹水 // 弓)
  - 號 (口尸 // 卜心 // 山)
  - 戲 (卜廿 // 戈) - double unit

- 隹 (人土) - turkey
  - 應 (戈 // 人土 // 心) - triple unit
  - 集 (人土 // 木)
  - 唯 (口 // 人土)
- 隺 (人月土) - coop
  - 鶴 (人土 // 竹日火)
  - 在 (大中土)
- 雚 (廿 // 口口 // 土)
  - 觀 (廿土 // 月山山)

- 住 (人 // 卜土)

- 隻 (人土水) - vessel
  - 護 (卜口 // 廿人水)
  - 獲 (大竹 // 廿人水)
- 雙 (人土水)
  - 双 (難水水)

- 雚 (廿 // 口口 // 土) - pegasus
  - 觀 (廿土 // 月山山)
  - 歡 (廿土 // 弓人)
  - 權 (木 // 廿 // 口土)

- 亡 (卜女) - deceased
  - 吂 (卜女 // 口)
  - 忘 (卜女 // 心)
  - 罔 (月 // 廿 // 卜女)
- 亡 on top of 口 (吂) is an exception.
- 望 (卜月 // 竹土) - double unit; F+L // F2+L
- 荒 (廿 // 卜女 // 山) - triple unit

- 气 (人一弓)

- 黑 (田土 // 火) - double unit

- 戠 (卜日戈) - kazoo
  - 識 (卜口 // 卜戈 // 日) - triple unit
  - 織 (女火 // 卜戈 // 日) - triple unit

- 靈 (一月 // 口口 // 人) - triple unit, spirits, soul
- 巫 (一人人) - witch

- 巔 (山 // 十金 // 金) - triple unit

- 剩 (竹心 // 中 // 弓)
  - 乘 (竹木中心)

## Rime input

user folder:

- windows黄鼠狼：%APPDATA%\Rime
- mac松鼠：~/library/rime
- ibus：~/.config/ibus/rime

## Resources

tool hán nôm của ĐH khoa học tự nhiên: <https://tools.clc.hcmus.edu.vn/>

Hội Nghiên cứu và Ứng dụng Hán Nôm 會研究吧應用漢喃: <https://hannom-rcv.org/index.php?uiLang=vi>

 Bộ Gõ WinVNKey: <https://winvnkey.sourceforge.net/hannom/SoLuocCachNhapChuHanNom-toanbo.htm>

 <https://rime.im/>
