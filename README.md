<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8" />
  <title>留学生文化适应指数测量平台</title>
  <!-- 引入 Chart.js，用来画雷达图 -->
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <style>
    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }

    body {
      font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
      background: #f3f4f6;
      color: #111827;
      padding: 20px;
      display: flex;
      justify-content: center;
    }

    .app {
      width: 100%;
      max-width: 1100px;
      background: #ffffff;
      border-radius: 16px;
      box-shadow: 0 10px 25px rgba(0,0,0,0.08);
      overflow: hidden;
    }

    .header {
      padding: 20px 24px;
      background: linear-gradient(135deg, #2563eb, #1d4ed8);
      color: #ffffff;
    }

    .header h1 {
      font-size: 22px;
      margin-bottom: 8px;
    }

    .header p {
      font-size: 13px;
      opacity: 0.9;
      line-height: 1.5;
    }

    .main {
      display: grid;
      grid-template-columns: 2fr 1.4fr;
      gap: 0;
    }

    .left {
      padding: 20px 24px 16px;
      border-right: 1px solid #e5e7eb;
    }

    .right {
      padding: 20px 20px 16px;
      background: #f9fafb;
    }

    .section-title {
      font-size: 16px;
      font-weight: 600;
      margin-bottom: 6px;
      color: #111827;
    }

    .section-sub {
      font-size: 12px;
      color: #6b7280;
      margin-bottom: 12px;
    }

    .scale-tip {
      font-size: 12px;
      color: #4b5563;
      padding: 8px 10px;
      background: #eff6ff;
      border-radius: 8px;
      border: 1px solid #dbeafe;
      margin-bottom: 14px;
      line-height: 1.5;
    }

    .question-group {
      margin-bottom: 18px;
      padding: 12px 12px 10px;
      border-radius: 12px;
      border: 1px solid #e5e7eb;
      background: #f9fafb;
    }

    .group-title {
      font-size: 14px;
      font-weight: 600;
      margin-bottom: 4px;
      color: #111827;
    }

    .group-sub {
      font-size: 12px;
      color: #6b7280;
      margin-bottom: 6px;
    }

    .question {
      margin-bottom: 8px;
      font-size: 13px;
      line-height: 1.4;
    }

    .question label {
      display: block;
      margin-bottom: 4px;
    }

    .options {
      display: flex;
      gap: 6px;
      flex-wrap: wrap;
      font-size: 11px;
    }

    .options label {
      display: inline-flex;
      align-items: center;
      gap: 2px;
      padding: 3px 6px;
      border-radius: 999px;
      border: 1px solid #e5e7eb;
      background: #ffffff;
      cursor: pointer;
    }

    .options input {
      cursor: pointer;
    }

    .submit-row {
      margin-top: 10px;
      display: flex;
      justify-content: flex-end;
      gap: 8px;
      align-items: center;
    }

    .submit-row small {
      font-size: 11px;
      color: #6b7280;
    }

    button {
      border: none;
      padding: 8px 16px;
      border-radius: 999px;
      cursor: pointer;
      font-size: 13px;
      font-weight: 600;
      background: #2563eb;
      color: #ffffff;
    }

    button:disabled {
      opacity: 0.5;
      cursor: default;
    }

    .result-card {
      background: #ffffff;
      border-radius: 12px;
      border: 1px solid #e5e7eb;
      padding: 12px 12px 8px;
      margin-bottom: 10px;
    }

    .result-card h3 {
      font-size: 14px;
      margin-bottom: 6px;
    }

    .score-row {
      font-size: 13px;
      margin-bottom: 4px;
      display: flex;
      justify-content: space-between;
      align-items: baseline;
    }

    .score-label {
      color: #374151;
    }

    .score-value {
      font-weight: 600;
      color: #111827;
    }

    .score-tag {
      font-size: 11px;
      padding: 2px 8px;
      border-radius: 999px;
      background: #e5e7eb;
      color: #374151;
      margin-left: 6px;
    }

    .score-tag.good {
      background: #dcfce7;
      color: #166534;
    }

    .score-tag.mid {
      background: #fef9c3;
      color: #854d0e;
    }

    .score-tag.low {
      background: #fee2e2;
      color: #b91c1c;
    }

    .advice {
      font-size: 12px;
      color: #4b5563;
      line-height: 1.5;
      margin-top: 4px;
      white-space: pre-wrap;
    }

    .chart-box {
      background: #ffffff;
      border-radius: 12px;
      border: 1px solid #e5e7eb;
      padding: 10px;
      margin-bottom: 10px;
    }

    .chart-box h3 {
      font-size: 13px;
      margin-bottom: 4px;
    }

    .chart-box p {
      font-size: 11px;
      color: #6b7280;
      margin-bottom: 6px;
    }

    .extra-note {
      font-size: 11px;
      color: #6b7280;
      margin-top: 6px;
      line-height: 1.5;
    }

    .placeholder {
      font-size: 12px;
      color: #9ca3af;
    }

    @media (max-width: 900px) {
      .main {
        grid-template-columns: 1fr;
      }
      .left {
        border-right: none;
        border-bottom: 1px solid #e5e7eb;
      }
    }

    @media (max-width: 600px) {
      .header h1 {
        font-size: 18px;
      }
      body {
        padding: 10px;
      }
    }
  </style>
</head>
<body>
  <div class="app">
    <div class="header">
      <h1>留学生文化适应指数测量平台</h1>
      <p>
        面向在韩国生活的留学生，评估在语言、学业、社交与生活情绪方面的适应程度，给出基于统计结果的个性化建议。<br />
        量表为 1–5 分：1 = 完全不同意，5 = 完全同意。
      </p>
    </div>

    <div class="main">
      <!-- 左侧：问卷 -->
      <div class="left">
        <div class="section-title">文化适应问卷（简版 · 20 题）</div>
        <div class="section-sub">
          请根据你最近 1～3 个月在韩国的真实状态作答。所有问题均无“对错”，只反映你目前的感受。
        </div>

        <div class="scale-tip">
          <strong>评分方式（Likert 量表）：</strong><br />
          1 = 完全不同意 · 2 = 比较不同意 · 3 = 一般 · 4 = 比较同意 · 5 = 完全同意
        </div>

        <form id="adaptForm">
          <!-- 语言适应 -->
          <div class="question-group">
            <div class="group-title">A. 语言适应（韩语）</div>
            <div class="group-sub">日常交流、课堂理解、自我表达的难易程度。</div>

            <div class="question">
              <label>1. 我能用韩语处理基本生活事务（买东西、问路、点餐等）。</label>
              <div class="options" data-name="lang1"></div>
            </div>
            <div class="question">
              <label>2. 在银行、医院或行政机关，我用韩语沟通时基本能说清自己的需求。</label>
              <div class="options" data-name="lang2"></div>
            </div>
            <div class="question">
              <label>3. 我在课堂上听懂大部分韩语授课内容。</label>
              <div class="options" data-name="lang3"></div>
            </div>
            <div class="question">
              <label>4. 我敢在课堂或小组讨论中用韩语发言。</label>
              <div class="options" data-name="lang4"></div>
            </div>
            <div class="question">
              <label>5. 遇到陌生情境时，我用韩语表达并不会特别紧张。</label>
              <div class="options" data-name="lang5"></div>
            </div>
          </div>

          <!-- 学业/校园适应 -->
          <div class="question-group">
            <div class="group-title">B. 学业 / 校园适应</div>
            <div class="group-sub">课程理解、作业应对、对学校制度的熟悉程度。</div>

            <div class="question">
              <label>6. 我基本能理解各门课程的要求与评分方式。</label>
              <div class="options" data-name="acad1"></div>
            </div>
            <div class="question">
              <label>7. 我知道如何选课、退课，以及在哪里查询学籍相关信息。</label>
              <div class="options" data-name="acad2"></div>
            </div>
            <div class="question">
              <label>8. 我能按时完成大部分作业或报告。</label>
              <div class="options" data-name="acad3"></div>
            </div>
            <div class="question">
              <label>9. 遇到学业困难时，我知道可以向谁求助（教授、助教、同学等）。</label>
              <div class="options" data-name="acad4"></div>
            </div>
            <div class="question">
              <label>10. 我觉得自己已经逐渐适应韩国大学/研究生院的学习节奏。</label>
              <div class="options" data-name="acad5"></div>
            </div>
          </div>

          <!-- 社交适应 -->
          <div class="question-group">
            <div class="group-title">C. 社交适应</div>
            <div class="group-sub">朋友、人际关系、参加活动的舒适程度。</div>

            <div class="question">
              <label>11. 我在韩国有可以一起吃饭或聊天的朋友。</label>
              <div class="options" data-name="soc1"></div>
            </div>
            <div class="question">
              <label>12. 面对韩国同学或老师，我通常不会感到“格格不入”。</label>
              <div class="options" data-name="soc2"></div>
            </div>
            <div class="question">
              <label>13. 我愿意参加学校或当地社区的线下活动（社团、节日、说明会等）。</label>
              <div class="options" data-name="soc3"></div>
            </div>
            <div class="question">
              <label>14. 和不同文化背景的人相处时，我能逐渐找到适合自己的相处方式。</label>
              <div class="options" data-name="soc4"></div>
            </div>
            <div class="question">
              <label>15. 当我遇到困难时，我觉得身边“有人可以求助”。</label>
              <div class="options" data-name="soc5"></div>
            </div>
          </div>

          <!-- 情绪 / 生活适应 -->
          <div class="question-group">
            <div class="group-title">D. 情绪与生活适应</div>
            <div class="group-sub">压力、孤独感、生活节奏与整体幸福感。（部分为反向题）</div>

            <div class="question">
              <label>16. 我经常因为学业、金钱或未来而感到严重压力。（反向题）</label>
              <div class="options" data-name="life1"></div>
            </div>
            <div class="question">
              <label>17. 我经常感到孤独或与周围环境格格不入。（反向题）</label>
              <div class="options" data-name="life2"></div>
            </div>
            <div class="question">
              <label>18. 我的作息（睡眠、饮食）在韩国基本比较规律。</label>
              <div class="options" data-name="life3"></div>
            </div>
            <div class="question">
              <label>19. 即使遇到困难，我仍然觉得“在韩国生活是值得的”。</label>
              <div class="options" data-name="life4"></div>
            </div>
            <div class="question">
              <label>20. 总体来说，我对目前在韩国的生活比较满意。</label>
              <div class="options" data-name="life5"></div>
            </div>
          </div>

          <div class="submit-row">
            <small>※ 所有题目均为必答，请根据直觉作答即可。</small>
            <button type="submit">生成文化适应指数</button>
          </div>
        </form>
      </div>

      <!-- 右侧：结果 & 图表 & 建议 -->
      <div class="right">
        <div class="section-title">测量结果 & 统计可视化</div>
        <div class="section-sub">
          提交问卷后，将自动计算 4 个维度指数与总文化适应指数，并生成雷达图与文字建议。
        </div>

        <div id="resultArea" class="placeholder">
          还未填写问卷。完成左侧 20 题后，点击「生成文化适应指数」，这里将显示你的结果与建议。
        </div>

        <div id="fullResult" style="display:none;">
          <div class="result-card">
            <h3>总体文化适应指数（0–100）</h3>
            <div class="score-row">
              <span class="score-label">总指数 / Total Adaptation Index</span>
              <span class="score-value" id="totalScoreText">--</span>
            </div>
            <div class="advice" id="totalAdvice"></div>
          </div>

          <div class="chart-box">
            <h3>四维度适应雷达图</h3>
            <p>维度：语言 · 学业 · 社交 · 情绪/生活。越接近外圈表示该方面适应度越高。</p>
            <canvas id="radarChart" height="230"></canvas>
            <div class="extra-note">
              ※ 本图可用于课堂展示，说明你如何将统计测量结果可视化并用于解释“文化适应”的多维结构。
            </div>
          </div>

          <div class="result-card">
            <h3>分维度指数 & 个性化建议</h3>

            <div class="score-row">
              <span class="score-label">语言适应指数</span>
              <span>
                <span class="score-value" id="langScoreText">--</span>
                <span class="score-tag" id="langTag">---</span>
              </span>
            </div>
            <div class="advice" id="langAdvice"></div>

            <div class="score-row" style="margin-top:6px;">
              <span class="score-label">学业 / 校园适应指数</span>
              <span>
                <span class="score-value" id="acadScoreText">--</span>
                <span class="score-tag" id="acadTag">---</span>
              </span>
            </div>
            <div class="advice" id="acadAdvice"></div>

            <div class="score-row" style="margin-top:6px;">
              <span class="score-label">社交适应指数</span>
              <span>
                <span class="score-value" id="socScoreText">--</span>
                <span class="score-tag" id="socTag">---</span>
              </span>
            </div>
            <div class="advice" id="socAdvice"></div>

            <div class="score-row" style="margin-top:6px;">
              <span class="score-label">情绪 / 生活适应指数</span>
              <span>
                <span class="score-value" id="lifeScoreText">--</span>
                <span class="score-tag" id="lifeTag">---</span>
              </span>
            </div>
            <div class="advice" id="lifeAdvice"></div>
          </div>

          <div class="extra-note">
            ※ 从统计学角度，你可以在报告中说明：<br />
            · 本网站使用 Likert 量表对文化适应进行多维度量化；<br />
            · 将题目平均分换算为 0–100 指数；<br />
            · 可进一步对多名被试数据做描述统计、相关分析、因子分析、回归分析等。
          </div>
        </div>
      </div>
    </div>
  </div>

  <script>
    // 生成 1-5 分的选项按钮（避免每题手写）
    function createOptions(container, name) {
      const texts = ["1 完全不同意", "2 比较不同意", "3 一般", "4 比较同意", "5 完全同意"];
      for (let i = 1; i <= 5; i++) {
        const label = document.createElement("label");
        const input = document.createElement("input");
        input.type = "radio";
        input.name = name;
        input.value = i;
        label.appendChild(input);
        label.appendChild(document.createTextNode(texts[i - 1]));
        container.appendChild(label);
      }
    }

    // 初始化所有题目的选项
    document.querySelectorAll(".options").forEach(div => {
      const name = div.getAttribute("data-name");
      createOptions(div, name);
    });

    const form = document.getElementById("adaptForm");
    const resultArea = document.getElementById("resultArea");
    const fullResult = document.getElementById("fullResult");

    let radarChart = null;

    function getGroupScore(names, reverseFlags) {
      let sum = 0;
      let count = 0;
      for (let i = 0; i < names.length; i++) {
        const name = names[i];
        const checked = document.querySelector(`input[name="${name}"]:checked`);
        if (!checked) return null; // 有未选题目，返回 null
        let v = parseInt(checked.value, 10);
        // 反向题：1<->5, 2<->4, 3->3
        if (reverseFlags && reverseFlags[i]) {
          v = 6 - v;
        }
        sum += v;
        count++;
      }
      const avg = sum / count;
      const index = Math.round(avg * 20); // 转成 0–100 指数
      return index;
    }

    function getTagAndAdvice(score, type) {
      let tagClass = "mid";
      let tagText = "中等";
      let advice = "";

      if (score >= 80) {
        tagClass = "good";
        tagText = "适应良好";
      } else if (score >= 60) {
        tagClass = "mid";
        tagText = "适应中等";
      } else {
        tagClass = "low";
        tagText = "适应偏低";
      }

      if (type === "lang") {
        if (score >= 80) {
          advice =
            "你的韩语适应度处于较高水平，说明你已经可以比较自如地用韩语处理学习和生活事务。\n" +
            "可以尝试：更多用韩语参与讨论、写邮件、参加演讲或发表，向“自然表达”和“专业表达”方向升级。";
        } else if (score >= 60) {
          advice =
            "你的韩语基础已能支持日常生活和部分学业，但在复杂场景（银行、医院、课堂提问）中可能仍有不安。\n" +
            "建议：每天固定 15–30 分钟听力/口语输入；在实际场景中尝试多说一句，比如问路时多加一句确认表达。";
        } else {
          advice =
            "目前你的语言适应指数偏低，说明韩语在一定程度上限制了你对生活和学习的掌控感。\n" +
            "建议：优先解决“生存韩语”（超市、银行、医院、公交等场景）；可以使用分场景对话练习网页、参加学校或市政府提供的韩国语课程。";
        }
      }

      if (type === "acad") {
        if (score >= 80) {
          advice =
            "你已经较好地适应了韩国的学业环境，说明在课程理解、作业、制度上都有一定经验。\n" +
            "可以进一步：尝试参与研究项目、学术发表或竞赛，把“适应”升级为“主动发展”。";
        } else if (score >= 60) {
          advice =
            "你对课程要求和节奏基本了解，但有时可能仍感到压力或不知从何下手。\n" +
            "建议：主动利用教授咨询时间、与同学组成学习小组、提前了解每学期的关键时间节点（作业、考试、选课）。";
        } else {
          advice =
            "目前你的学业适应指数偏低，说明你在课程理解、作业或制度方面可能存在困惑。\n" +
            "建议：整理一份“问题清单”，集中向导师/学长学姐请教；同时利用学校官网、国际处说明会等渠道获取信息。";
        }
      }

      if (type === "soc") {
        if (score >= 80) {
          advice =
            "你的社交适应度较高，说明你已经建立了一定的人际网络，在陌生文化环境中也能找到自己的位置。\n" +
            "可以考虑：作为“桥梁”，帮助其他新来的留学生适应，或者加入更多跨文化活动。";
        } else if (score >= 60) {
          advice =
            "你在社交方面处于“还可以”的状态，有一些朋友，也能参与部分活动，但可能偶尔仍感到距离感。\n" +
            "建议：每周给自己一个“小任务”，例如主动约一位同学喝咖啡、参加一次线下活动，慢慢扩大舒适圈。";
        } else {
          advice =
            "你的社交适应指数偏低，这并不代表你“有问题”，而是说明你可能比较孤立、缺少可以依靠的关系。\n" +
            "建议：\n- 先从一对一关系开始，而不是一下子融入大团体；\n- 利用同乡、学会、兴趣社团等相对熟悉的圈子作为过渡；\n- 允许自己慢慢来，不必和别人比较速度。";
        }
      }

      if (type === "life") {
        if (score >= 80) {
          advice =
            "你的情绪和生活适应度较高，说明你在压力管理、生活节奏和整体幸福感方面做得很好。\n" +
            "可以尝试：将目前有效的习惯（运动、作息、兴趣）固定下来，形成长期可持续的生活方式。";
        } else if (score >= 60) {
          advice =
            "你的生活与情绪状态大体稳定，但在高压时期可能会出现睡眠、情绪波动等问题。\n" +
            "建议：关注“预警信号”（失眠、暴饮暴食、持续焦虑等），在压力变大之前提前调整节奏、适当运动或寻求支持。";
        } else {
          advice =
            "目前情绪与生活适应指数偏低，可能说明你承受了较多压力或不适感。\n" +
            "建议：\n- 尝试规律作息和轻量运动（散步、拉伸等）；\n- 与信任的人（朋友、家人、导师）分享感受；\n- 如有持续严重的不安，可考虑寻求学校咨询中心或专业心理服务的帮助。\n\n※ 本测量仅用于自我觉察，不替代专业医疗或心理诊断。";
        }
      }

      return { tagClass, tagText, advice };
    }

    form.addEventListener("submit", function (e) {
      e.preventDefault();

      // 分维度计算指数
      const langScore = getGroupScore(
        ["lang1", "lang2", "lang3", "lang4", "lang5"]
      );
      const acadScore = getGroupScore(
        ["acad1", "acad2", "acad3", "acad4", "acad5"]
      );
      const socScore = getGroupScore(
        ["soc1", "soc2", "soc3", "soc4", "soc5"]
      );
      const lifeScore = getGroupScore(
        ["life1", "life2", "life3", "life4", "life5"],
        // life1、life2 为反向题
        [true, true, false, false, false]
      );

      if (
        langScore === null ||
        acadScore === null ||
        socScore === null ||
        lifeScore === null
      ) {
        alert("有题目未作答，请检查所有问题是否都选择了 1–5 分。");
        return;
      }

      const totalScore = Math.round((langScore + acadScore + socScore + lifeScore) / 4);

      // 隐藏 placeholder，显示结果区域
      resultArea.style.display = "none";
      fullResult.style.display = "block";

      // 总指数
      document.getElementById("totalScoreText").textContent = totalScore + " 分";

      let totalAdviceText = "";
      if (totalScore >= 80) {
        totalAdviceText =
          "整体来看，你的文化适应指数较高，说明你已经在语言、学业、社交与生活方面建立起相对稳定的平衡。\n" +
          "你可以在报告或发表中，将自己视为“较好适应者”的一例，从中总结哪些策略对你有效（如主动沟通、时间管理、参加活动等）。";
      } else if (totalScore >= 60) {
        totalAdviceText =
          "你的整体文化适应处于中等水平，既有已经适应良好的部分，也存在需要进一步调整的方面。\n" +
          "可以重点观察“最低的那个维度”，作为下一阶段优先改善的目标，例如：语言、社交或情绪管理。";
      } else {
        totalAdviceText =
          "你的整体文化适应指数偏低，这并不是“失败”，而是提醒你：当前的环境可能对你提出了较大的挑战。\n" +
          "建议在确保基本生活稳定的前提下，循序渐进地改善：先从生存韩语、规律作息、建立一两段信任关系开始。";
      }
      document.getElementById("totalAdvice").textContent = totalAdviceText;

      // 分维度标签与建议
      const langRes = getTagAndAdvice(langScore, "lang");
      const acadRes = getTagAndAdvice(acadScore, "acad");
      const socRes = getTagAndAdvice(socScore, "soc");
      const lifeRes = getTagAndAdvice(lifeScore, "life");

      document.getElementById("langScoreText").textContent = langScore + " 分";
      document.getElementById("acadScoreText").textContent = acadScore + " 分";
      document.getElementById("socScoreText").textContent = socScore + " 分";
      document.getElementById("lifeScoreText").textContent = lifeScore + " 分";

      function applyTag(tagId, res) {
        const el = document.getElementById(tagId);
        el.textContent = res.tagText;
        el.className = "score-tag " + res.tagClass;
      }

      applyTag("langTag", langRes);
      applyTag("acadTag", acadRes);
      applyTag("socTag", socRes);
      applyTag("lifeTag", lifeRes);

      document.getElementById("langAdvice").textContent = langRes.advice;
      document.getElementById("acadAdvice").textContent = acadRes.advice;
      document.getElementById("socAdvice").textContent = socRes.advice;
      document.getElementById("lifeAdvice").textContent = lifeRes.advice;

      // 雷达图
      const ctx = document.getElementById("radarChart").getContext("2d");
      if (radarChart) {
        radarChart.destroy();
      }
      radarChart = new Chart(ctx, {
        type: "radar",
        data: {
          labels: ["语言适应", "学业适应", "社交适应", "情绪/生活适应"],
          datasets: [
            {
              label: "文化适应指数",
              data: [langScore, acadScore, socScore, lifeScore],
              fill: true
            }
          ]
        },
        options: {
          responsive: true,
          maintainAspectRatio: false,
          scales: {
            r: {
              suggestedMin: 0,
              suggestedMax: 100,
              ticks: {
                stepSize: 20
              }
            }
          },
          plugins: {
            legend: {
              display: false
            }
          }
        }
      });
    });
  </script>
</body>
</html>
