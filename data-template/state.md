# English Immersion State

mode: off

## 说明

这个文件是 `english-immersion` skill 的 ON/OFF 开关。

- `mode: on` → Claude 进入英语浸入模式：英文回复、记录输入、捕捉错误、session-end 批量复盘
- `mode: off` → 正常模式，skill 不干扰

## 切换方式

- **开启**：对 Claude 说 "english mode on" / "学英语" / "练英语" / 任何 `/pte-*` 命令会自动开
- **关闭**：说 "english mode off" / "中文" / 任何中文内容生产 skill 会自动关

## 历史切换记录

（Claude 切换时在这里追加一行：YYYY-MM-DD HH:MM → on/off → 触发原因）

---

## Sliding Anchor (pte skill)

> 由 `pte` skill 维护。english-immersion 在 ON 模式下读这几个字段，决定回复中"未达阈值规则"的示范密度。

- exam_date: null
- current_level: null
- target_score: null
- set_at: null
- notes:
