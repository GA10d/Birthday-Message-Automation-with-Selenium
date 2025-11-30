下面是一个**课程项目风格、比较详细的中英双语 README**，你可以直接保存为 `README.md` 用在 GitHub 上。这份描述是根据 `main.html` 里实际 Selenium 代码的行为整理出来的：

---

````markdown
# Course Project: Web Automation for Personalized Birthday Greetings  
# 课程项目：基于 Selenium 的生日祝福文案自动化编辑

> 🎓 This repository is organized as a **course project** report for a web automation / Python / software engineering related class.  
> 🎓 本仓库以**课程项目**形式整理，适用于 Web 自动化 / Python / 软件工程相关课程的作业或展示。

---

## 1. Project Background & Motivation | 项目背景与动机

### English

In many non-profit or membership-based organizations, staff need to send personalized **birthday greetings** to a large number of donors or members.  
If the content is edited manually in an online editor (for example, a rich text editor in a web system), this process:

- Is repetitive and time-consuming
- Is error-prone (wrong name, wrong date)
- Does not scale when the number of people grows

This project demonstrates how to use **Python + Selenium WebDriver** to automatically locate and edit birthday message content in a web-based editor. The main goal is to **replace template text with personalized content (name + greeting + date)** and apply consistent style (color + bold), which can be used as part of a larger birthday-email automation pipeline.

### 中文

在很多公益组织或会员体系中，工作人员需要给大量“月捐伙伴”或会员发送**个性化生日祝福**。  
如果完全在网页富文本编辑器里手动改：

- 非常重复、耗时
- 容易出错（姓名、日期、格式）
- 人数一多难以扩展

本项目展示了如何使用 **Python + Selenium WebDriver** 对网页上的生日祝福文案进行**自动定位与编辑**：  
自动从网页右侧编辑器中找到对应段落，写入“真实姓名 + 生日祝福 + 日期”，并统一调整颜色和加粗格式，可作为**生日邮件自动化系统**中的一个功能模块。

---

## 2. Project Objectives | 项目目标

### English

1. Use **Selenium** to control a real browser and operate on a web-based editor.
2. Automatically locate specific `<p>` elements in the birthday template, including:
   - The greeting line containing the donor’s name
   - The “Happy Birthday” line
   - The date line below the signature
3. Replace template texts with **personalized content** based on input parameters (`name`, `month`, `day`).
4. Apply a consistent visual style via JavaScript:
   - Change text color to `rgb(48,151,102)` (#309766)
   - Set `font-weight: bold`
5. Wrap the above logic into reusable helper functions for robustness and readability.

### 中文

1. 使用 **Selenium** 控制真实浏览器，自动操作网页上的富文本编辑器。  
2. 自动定位生日模板中的关键 `<p>` 段落，包括：
   - 含有称呼“亲爱的月捐伙伴”的那一行
   - 含有“PEER祝你生日快乐”的生日祝福行
   - 署名下面、最后一个包含“年/月”的日期行
3. 根据输入参数（`name`, `month`, `day`）自动生成**个性化文案**并写回网页。  
4. 通过 JavaScript 统一设置文本样式：
   - 字体颜色改为 `rgb(48,151,102)`（#309766 绿色）
   - 文本加粗（`font-weight: bold`）
5. 将控制流程封装成函数，增强**可读性与鲁棒性**。

---

## 3. Repository Structure | 仓库结构

```bash
.
├── main.html       # Jupyter Notebook 导出的静态 HTML 报告，展示完整代码与说明
└── README.md       # 本说明文件（课程项目风格）
````

> 如果你后续把 `.ipynb` 或 `.py` 脚本也上传，可以在这里继续补充相应文件结构。

---

## 4. System Overview | 系统概述

### English

The core logic is implemented inside a Jupyter Notebook and then exported as `main.html`. In that notebook, we:

* Import Selenium and related modules:

  * `selenium.webdriver.common.by.By`
  * `selenium.webdriver.support.ui.WebDriverWait`
  * `selenium.webdriver.support.expected_conditions as EC`
* Define helper functions to:

  * Wait for loading indicators to disappear
  * Click the real content block that contains the birthday message
  * Enter the iframe which hosts the right-side editor
  * Edit specific paragraphs by XPath and inject new HTML + CSS

All code is presented in notebook cells, with print logs (in Chinese) indicating each step during execution, such as:

* `"🖱️ 点击真正的内容模块（生日文案）."`
* `"✏️ 在右侧编辑器中写入真实内容..."`

### 中文

核心逻辑写在 Jupyter Notebook 中，并导出为 `main.html` 报告。Notebook 中主要包括：

* 导入 Selenium 及相关模块：

  * `By`（定位方式）
  * `WebDriverWait`（显式等待）
  * `expected_conditions as EC`（等待条件）
* 定义若干工具函数，用于：

  * 等待加载动画消失
  * 点击左侧真正包含生日文案的内容块
  * 进入右侧编辑器所在的 `iframe`
  * 通过 XPath 精确找到 `<p>` 段落并注入新 HTML + 样式

代码单元会输出中文日志，方便调试和教学，例如：

* “🖱️ 点击真正的内容模块（生日文案）.”
* “✏️ 在右侧编辑器中写入真实内容…”

---

## 5. Core Functions | 核心函数说明

### 5.1 `click_left_block()` – Select the Content Block

### 选择生日文案内容块

**English**

This function finds the paragraph that contains both `"亲爱的月捐伙伴"` and `"生日"` and then goes up the DOM tree to find the nearest clickable container (the real content block). It ensures we click on the correct module containing the birthday message.

**中文**

`click_left_block()` 通过 XPath 寻找同时包含“亲爱的月捐伙伴”和“生日”的 `<p>` 段落，然后**向上寻找最近的可点击大容器**（真正的内容块），最终对该块执行点击操作，确保选中的是生日文案对应的模块。

---

### 5.2 `edit_right_editor(name, month, day)` – Edit Text in the Right Editor

### 在右侧编辑器中自动写入文案

**English**

This is the main editing function:

1. Wait for the iframe of the right editor and switch into it.
2. Locate three key paragraphs:

   * The greeting line containing `"亲爱的月捐伙伴"`
   * The birthday line containing `"生日"` and `"PEER祝你生日快乐"`
   * The last date line that contains both `"年"` and `"月"` (usually under the signature)
3. Define a helper `set_p(el, text)` that:

   * Uses `driver.execute_script` to set `innerHTML = text`
   * Changes `style.color` to green
   * Sets `style.fontWeight = 'bold'`
4. Generate personalized strings using `name`, `month`, `day` and the current year:

   * `f"亲爱的月捐伙伴 {name}，"`
   * Formatted date string like `"2024年12月07日"`
5. Write these strings back into the corresponding paragraphs with styling.

**中文**

这是整个项目的核心函数，主要流程：

1. 等待右侧编辑器的 `iframe` 出现，并 `switch_to.frame(iframe)` 进入内部。
2. 依次找到三个关键段落：

   * 含有“亲爱的月捐伙伴”的称呼行
   * 含有“生日”和“PEER祝你生日快乐”的祝福行
   * 署名下方、**最后一个既含“年”又含“月”** 的日期行
3. 定义工具函数 `set_p(el, text)`：

   * 通过 `execute_script` 修改 `innerHTML`
   * 设置字体颜色为绿色 `rgb(48,151,102)`
   * 设置加粗 `fontWeight = 'bold'`
4. 使用 `name`, `month`, `day` 和当前年份构造个性化字符串，例如：

   * `f"亲爱的月捐伙伴 {name}，"`
   * `"2024年12月07日"` 这样格式的日期
5. 调用 `set_p` 将这些文本写回到对应 `<p>` 段落中，实现**自动内容替换 + 统一样式**。

---

### 5.3 `click_with_retry(...)` – Robust Clicking with Retries

### 带重试机制的点击封装

**English**

To make the automation more robust, a helper function wraps the click operation in a loop with multiple retries:

* Try to locate and click an element
* If an exception occurs (e.g. element not clickable, temporary overlay), print a message and sleep for a few seconds
* After several failed attempts, raise `TimeoutException("XXX 多次点击失败")`

This pattern is common in production-grade Selenium scripts.

**中文**

为了提高脚本鲁棒性，项目中还封装了一个**带重试机制的点击函数**：

* 循环尝试查找并点击指定元素
* 如果失败（元素暂时不可点击、被遮挡等），打印错误信息并 `sleep(3)` 后重试
* 超过最大重试次数仍失败时，抛出 `TimeoutException("XXX 多次点击失败")`，方便上层捕获与排查日志

---

## 6. How to Use This Repository | 如何使用本仓库

### 6.1 View the Report (HTML) | 查看 HTML 报告

**English**

1. Clone or download this repository.
2. Open `main.html` in any modern browser (Chrome, Firefox, Edge, etc.).
3. You can:

   * Read through the code cells and comments
   * Follow the printed logs to understand each step
   * Use it as a reference for your own Selenium projects

**中文**

1. 克隆或下载本仓库；
2. 使用任意现代浏览器打开 `main.html`；
3. 你可以：

   * 通读每一个代码单元和中文注释
   * 结合打印日志理解自动化每一步在做什么
   * 把这里的代码结构作为自己写 Selenium 项目的参考模板

---

### 6.2 (Optional) Re-Run the Notebook | （可选）重新运行 Notebook

> 🔧 Note: The current repository only contains the exported HTML.
> If you also keep the original `.ipynb` or `.py` file locally, you can:

**English**

1. Open the notebook in Jupyter / VS Code.
2. Install dependencies:

   ```bash
   pip install selenium
   ```
3. Configure your WebDriver (e.g. ChromeDriver).
4. Modify the `name`, `month`, `day` parameters as needed.
5. Run the cells and observe how the browser is controlled automatically.

**中文**

> 当前仓库只包含导出的 HTML。如果你本地还保留 `.ipynb` 或 `.py` 文件，可以：

1. 在 Jupyter / VS Code 中打开原始 Notebook；
2. 安装依赖：

   ```bash
   pip install selenium
   ```
3. 配置浏览器驱动（如 ChromeDriver）；
4. 根据需要修改传入的 `name`, `month`, `day` 等参数；
5. 依次运行代码单元，观察浏览器自动点击、切换 iframe、修改生日文案的过程。

---

## 7. Limitations & Future Work | 当前局限与后续改进

### English

* The XPath rules are highly **template-specific** (e.g. rely on Chinese phrases like `"亲爱的月捐伙伴"` and `"PEER祝你生日快乐"`). If the template changes, the script must be updated.
* Currently focuses on **editing content inside the editor**. A full production system would also:

  * Read user info (name, birthday) from a database or Excel file
  * Loop through all users and send emails automatically
  * Handle login, navigation, and error reporting in a more complete way

### 中文

* 目前的 XPath 规则高度依赖具体模板文本（例如“亲爱的月捐伙伴”“PEER祝你生日快乐”等），一旦网页文案调整，需要同步更新 XPath。
* 本项目更偏向展示**“如何自动编辑右侧生日文案”**，如果要扩展成完整系统，还可以进一步加入：

  * 从 Excel / 数据库中批量读取姓名和生日信息
  * 循环处理多位用户并批量发送邮件
  * 完整的登录流程、页面导航、异常日志系统等

---

## 8. Course Reflection (Optional Section) | 课程总结（可选）

> 如果作为课程报告，你可以在这里写一些个人反思，这里给出一个模板。

### English (Example)

Through this project, I practiced how to combine **web automation tools (Selenium)** with real-world workflow requirements (sending personalized birthday greetings). I learned:

* How to design robust XPath selectors
* How to handle dynamic content, iframes, and loading states
* How to wrap Selenium operations into reusable, well-structured functions

### 中文（示例）

通过本次课程项目，我将 **Selenium Web 自动化** 与实际业务场景（发送个性化生日祝福）结合起来，主要收获包括：

* 学会了如何设计相对鲁棒的 XPath 定位规则
* 理解了处理 iframe、动态加载页面和等待条件的常见方法
* 练习了将自动化步骤封装成结构清晰、可复用的函数，提高代码可维护性

---

## 9. Author | 作者信息

```text
Author / 作者：Gustav Guo 
Email：zg2567@columbia.eddu
NGO / 项目：PEER
Institution / 学校：Columbia University
```

---

Happy Coding & Automation 🎂
祝自动化愉快，文案再也不用手动改啦 🎉

```
