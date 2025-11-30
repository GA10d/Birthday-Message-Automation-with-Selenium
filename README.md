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
