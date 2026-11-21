# 当前设置备份 / Restore guide

这两个文件是这台机器上 `dsh-model-proxy` **实际生效的设置**（导出时间 2026-11-21），
用途是重装 DSH 之后把它原样恢复回来。

| 文件 | 内容 | 怎么用 |
| --- | --- | --- |
| `config.current.yml` | 与 `profiles/<profile>/cordis.patch.yml` 里同格式的完整条目 | 重装后**整段粘贴**回 profile 的 `cordis.patch.yml` |
| `config.current.json` | 同一份设置的扁平对象（对应插件 Config schema） | 在「设置 → 模型代理」里照着填，或喂给脚本 |

## 当前值一览

| 字段 | 值 |
| --- | --- |
| `mode` | `custom` |
| `proxyUri` | `socks5h://127.0.0.1:7890` |
| `proxyType` | `socks5h` |
| `proxyHosts` | 43 条（含 `*.nachoneko.chat`、`*.lmr-leavingnow.cn.mt` 等） |
| `excludeHosts` | 2 条（`*.deepseek.com`、`*.siliconflow.cn`） |
| `logLevel` | `info` |
| `schemaVersion` | `2` |

## 恢复步骤（重装 DSH 后）

1. 装回插件：

   ```bash
   dsh plugin --profile <profile> add github:TNTChinaAAA/dsh-model-proxy
   ```

2. 打开 `$DSH_HOME/profiles/<profile>/cordis.patch.yml`，把 `config.current.yml` 的**全文**
   追加到文件末尾（它是顶层列表的一个元素，缩进必须保持，前面不要空行缩进）。

3. 重启 DSH。设置页「模型代理」里应看到 43 条域名、排除 2 条、出口 `socks5h://127.0.0.1:7890`。

4. 想确认真的生效：访问 `http://127.0.0.1:<port>/dsh-model-proxy/status`，
   `ruleCount` 应是 `45`（43 条转发 + 2 条排除），`mode` 为 `custom`。

> 注意：这套配置把 `*.deepseek.com` 放在排除表里 —— 走 DeepSeek 官方接口时不经过代理。
> 换机器时如果本机没有 `127.0.0.1:7890` 的 SOCKS5 代理，先把 `proxyUri` 改成你自己的地址。
