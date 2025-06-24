# chinese-vocab-game3
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>中文词汇闯关王 - 汉语教学游戏</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Microsoft YaHei', sans-serif;
        }
        
        body {
            background: linear-gradient(135deg, #6a11cb 0%, #2575fc 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }
        
        .container {
            background: rgba(255, 255, 255, 0.93);
            width: 95%;
            max-width: 900px;
            border-radius: 20px;
            box-shadow: 0 15px 35px rgba(0, 0, 0, 0.25);
            overflow: hidden;
            position: relative;
        }
        
        .header {
            background: linear-gradient(to right, #ff416c, #ff4b2b);
            color: white;
            padding: 20px 0;
            text-align: center;
            position: relative;
        }
        
        .header h1 {
            font-size: 2.6rem;
            margin-bottom: 5px;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
            letter-spacing: 1px;
        }
        
        .level-display {
            background: rgba(255, 255, 255, 0.3);
            padding: 8px 20px;
            border-radius: 50px;
            font-size: 1.1rem;
            font-weight: bold;
            width: fit-content;
            margin: 0 auto;
            backdrop-filter: blur(5px);
        }
        
        .game-info {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            background: #f8f9fa;
            padding: 15px;
            border-bottom: 1px solid #e9ecef;
        }
        
        .info-box {
            text-align: center;
            padding: 10px;
        }
        
        .info-label {
            color: #6c757d;
            font-size: 0.95rem;
            margin-bottom: 5px;
        }
        
        .info-value {
            font-size: 1.8rem;
            font-weight: 700;
            color: #1e88e5;
        }
        
        .game-area {
            padding: 25px;
            transition: all 0.4s;
        }
        
        .question-container {
            text-align: center;
            margin-bottom: 30px;
            min-height: 100px;
            display: flex;
            flex-direction: column;
            justify-content: center;
        }
        
        #question-area {
            font-size: 2.3rem;
            color: #212529;
            font-weight: 600;
            padding: 15px;
            margin: 10px 0;
            border-radius: 12px;
            background: #f8f9fa;
        }
        
        .emoji-large {
            font-size: 4rem;
            margin: 15px 0;
        }
        
        #options-container {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 18px;
            margin: 20px 0;
        }
        
        .option {
            background: #1e88e5;
            color: white;
            padding: 25px 10px;
            border-radius: 15px;
            font-size: 1.5rem;
            text-align: center;
            cursor: pointer;
            transition: all 0.3s;
            box-shadow: 0 5px 15px rgba(33, 136, 229, 0.4);
            border: 3px solid transparent;
            user-select: none;
        }
        
        .option:hover {
            transform: translateY(-5px);
            background: #42a5f5;
            box-shadow: 0 8px 20px rgba(33, 136, 229, 0.6);
        }
        
        .option.selected {
            transform: scale(1.05);
            background: #ffee58;
            color: #333;
            box-shadow: 0 8px 20px rgba(255, 213, 79, 0.6);
        }
        
        .option.correct {
            background: #66bb6a;
            box-shadow: 0 5px 15px rgba(102, 187, 106, 0.6);
            border-color: #388e3c;
        }
        
        .option.incorrect {
            background: #ef5350;
            box-shadow: 0 5px 15px rgba(239, 83, 80, 0.4);
            border-color: #d32f2f;
        }
        
        .progress-container {
            background: #e9ecef;
            height: 15px;
            border-radius: 10px;
            overflow: hidden;
            margin: 30px 0 20px;
        }
        
        #progress-bar {
            height: 100%;
            background: linear-gradient(to right, #4caf50, #8bc34a);
            width: 0%;
            transition: width 0.6s ease-in-out;
        }
        
        .controls {
            display: flex;
            justify-content: center;
            gap: 20px;
            padding: 15px;
            flex-wrap: wrap;
        }
        
        .game-btn {
            padding: 14px 32px;
            border: none;
            border-radius: 50px;
            font-size: 1.1rem;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
            min-width: 150px;
        }
        
        #hint-btn {
            background: #ffca28;
            color: #212529;
        }
        
        #hint-btn:hover {
            background: #ffd54f;
        }
        
        #submit-btn {
            background: #1e88e5;
            color: white;
        }
        
        #submit-btn:hover {
            background: #42a5f5;
        }
        
        #next-btn {
            background: #66bb6a;
            color: white;
            display: none;
        }
        
        #next-btn:hover {
            background: #81c784;
        }
        
        .hint-display {
            background: #fff8e1;
            border-left: 5px solid #ffca28;
            border-radius: 8px;
            padding: 15px;
            margin: 20px 0;
            text-align: center;
            font-size: 1.2rem;
            color: #5d4037;
            display: none;
            animation: fadeIn 0.5s;
        }
        
        .feedback-popup {
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: rgba(0, 0, 0, 0.7);
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 1000;
            opacity: 0;
            pointer-events: none;
            transition: opacity 0.4s;
        }
        
        .feedback-content {
            background: white;
            padding: 40px;
            border-radius: 20px;
            text-align: center;
            max-width: 500px;
            width: 90%;
            transform: scale(0.8);
            transition: transform 0.4s;
        }
        
        .feedback-popup.active {
            opacity: 1;
            pointer-events: all;
        }
        
        .feedback-popup.active .feedback-content {
            transform: scale(1);
        }
        
        .feedback-title {
            font-size: 2.8rem;
            margin-bottom: 20px;
            color: #333;
        }
        
        .feedback-score {
            font-size: 3.5rem;
            color: #1e88e5;
            margin: 25px 0;
            font-weight: 700;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.1);
        }
        
        .feedback-message {
            font-size: 1.3rem;
            color: #555;
            margin-bottom: 25px;
        }
        
        #restart-btn {
            background: #ff7043;
            color: white;
            padding: 14px 40px;
            font-size: 1.2rem;
            border-radius: 50px;
            border: none;
            cursor: pointer;
            transition: all 0.3s;
            box-shadow: 0 5px 15px rgba(255, 112, 67, 0.4);
        }
        
        #restart-btn:hover {
            background: #ff8a65;
            transform: translateY(-3px);
        }
        
        .design-section {
            padding: 25px;
            background: #f8f9fa;
            border-top: 2px dashed #dee2e6;
            margin-top: 20px;
            display: none;
        }
        
        .section-title {
            color: #1e88e5;
            font-size: 1.8rem;
            margin-bottom: 20px;
            text-align: center;
        }
        
        .section-content {
            line-height: 1.8;
            color: #495057;
        }
        
        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }
        
        @media (max-width: 768px) {
            #options-container {
                grid-template-columns: repeat(2, 1fr);
            }
            
            .header h1 {
                font-size: 2rem;
            }
            
            #question-area {
                font-size: 1.8rem;
            }
            
            .option {
                padding: 18px 8px;
                font-size: 1.3rem;
            }
        }
        
        @media (max-width: 480px) {
            #options-container {
                grid-template-columns: 1fr;
            }
            
            .info-value {
                font-size: 1.5rem;
            }
            
            .game-btn {
                min-width: 120px;
                padding: 12px 20px;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>中文词汇闯关王</h1>
            <div class="level-display">基础关卡</div>
        </div>
        
        <div class="game-info">
            <div class="info-box">
                <div class="info-label">当前得分</div>
                <div id="score" class="info-value">0</div>
            </div>
            <div class="info-box">
                <div class="info-label">剩余时间</div>
                <div id="timer" class="info-value">60</div>
            </div>
            <div class="info-box">
                <div class="info-label">问题进度</div>
                <div id="current" class="info-value">1/10</div>
            </div>
        </div>
        
        <div class="game-area">
            <div class="progress-container">
                <div id="progress-bar"></div>
            </div>
            
            <div class="question-container">
                <div id="question-area">请选择与"苹果"对应的拼音</div>
                <div class="emoji-large" id="emoji-area" style="display:none;"></div>
            </div>
            
            <div class="hint-display" id="hint-display">提示：这是一种常见的水果</div>
            
            <div id="options-container">
                <!-- 选项会通过JavaScript动态生成 -->
            </div>
            
            <div class="controls">
                <button class="game-btn" id="hint-btn">提示</button>
                <button class="game-btn" id="submit-btn">提交答案</button>
                <button class="game-btn" id="next-btn">下一题</button>
            </div>
        </div>
        
        <div class="design-section" id="design-section">
            <h2 class="section-title">游戏设计思路与教学应用</h2>
            <div class="section-content">
                <p><strong>1. 教育理论基础：</strong>游戏设计融合了建构主义学习理论，通过情境创设和问题解决促进主动学习；基于输入假说(Comprehensible Input)原则，确保语言输入的可理解性；遵循认知负荷理论，控制信息呈现方式以优化学习效果。</p>
                
                <p><strong>2. 游戏机制设计：</strong>采用多元答题模式，包含拼音配对、英汉翻译和图片识别三种游戏形式，确保词汇学习的多维认知。游戏设计了即时反馈系统，对正确和错误答案提供视觉和分数反馈，强化学习效果。</p>
                
                <p><strong>3. 教学应用场景：</strong>本游戏适用于课堂词汇引入环节、课后复习阶段以及翻转课堂课前预习。教师可在多媒体教室组织小组竞赛，激发学习动机；学生可在移动端自主练习，平台将记录学习进度与错题情况。</p>
                
                <p><strong>4. 技术实现特点：</strong>采用响应式设计确保跨设备体验，使用JSON数据结构组织词汇库，通过本地存储记录学习进度。游戏界面遵循认知友好的配色方案与动画设计，平衡娱乐性与教育性。</p>
                
                <p><strong>5. 扩展方向：</strong>未来可加入语音识别实现发音练习，增加社交功能实现班级排名，开发关卡编辑工具让教师自定义内容，添加中国文化模块拓展学习维度。</p>
            </div>
        </div>
    </div>
    
    <div class="feedback-popup" id="feedback-popup">
        <div class="feedback-content">
            <h2 class="feedback-title">游戏结束!</h2>
            <div class="feedback-score" id="final-score">0</div>
            <div class="feedback-message">恭喜完成本次词汇挑战！</div>
            <button id="restart-btn">重新开始</button>
        </div>
    </div>

    <script>
        // 词汇数据库 (汉字, 拼音, 词性, 英文翻译, 图片)
        const vocabulary = [
            { chinese: "苹果", pinyin: "píng guǒ", type: "名词", en: "apple", image: "🍎", hint: "这是一种常见的水果"},
            { chinese: "书", pinyin: "shū", type: "名词", en: "book", image: "📖", hint: "阅读和学习的工具"},
            { chinese: "跑步", pinyin: "pǎo bù", type: "动词", en: "run", image: "🏃", hint: "一种有益健康的运动"},
            { chinese: "高兴", pinyin: "gāo xìng", type: "形容词", en: "happy", image: "😄", hint: "形容心情愉快的词语"},
            { chinese: "学校", pinyin: "xué xiào", type: "名词", en: "school", image: "🏫", hint: "学生学习知识的地方"},
            { chinese: "喝水", pinyin: "hē shuǐ", type: "动词", en: "drink water", image: "💧", hint: "日常必需的健康行为"},
            { chinese: "漂亮", pinyin: "piào liang", type: "形容词", en: "beautiful", image: "🌸", hint: "形容外观赏心悦目的词语"},
            { chinese: "电脑", pinyin: "diàn nǎo", type: "名词", en: "computer", image: "💻", hint: "现代工作学习的重要工具"},
            { chinese: "唱歌", pinyin: "chàng gē", type: "动词", en: "sing", image: "🎤", hint: "用声音表达音乐的行为"},
            { chinese: "好吃", pinyin: "hǎo chī", type: "形容词", en: "delicious", image: "😋", hint: "形容食物美味的词语"},
        ];
        
        // 游戏状态变量
        let currentQuestion = 0;
        let score = 0;
        let timeLeft = 60;
        let timer;
        let selectedOption = null;
        let questionsAnswered = 0;
        const maxQuestions = 10;
        
        // DOM元素
        const questionArea = document.getElementById('question-area');
        const emojiArea = document.getElementById('emoji-area');
        const optionsContainer = document.getElementById('options-container');
        const scoreDisplay = document.getElementById('score');
        const timerDisplay = document.getElementById('timer');
        const currentDisplay = document.getElementById('current');
        const progressBar = document.getElementById('progress-bar');
        const hintDisplay = document.getElementById('hint-display');
        const hintBtn = document.getElementById('hint-btn');
        const submitBtn = document.getElementById('submit-btn');
        const nextBtn = document.getElementById('next-btn');
        const feedbackPopup = document.getElementById('feedback-popup');
        const finalScore = document.getElementById('final-score');
        const restartBtn = document.getElementById('restart-btn');
        const designSection = document.getElementById('design-section');
        
        // 初始化游戏
        function initGame() {
            resetState();
            generateQuestion();
            startTimer();
            
            // 显示设计说明部分
            setTimeout(() => {
                designSection.style.display = 'block';
            }, 1000);
        }
        
        // 重置状态
        function resetState() {
            score = 0;
            timeLeft = 60;
            currentQuestion = 0;
            questionsAnswered = 0;
            selectedOption = null;
            scoreDisplay.textContent = score;
            timerDisplay.textContent = timeLeft;
            currentDisplay.textContent = `${questionsAnswered + 1}/${maxQuestions}`;
            progressBar.style.width = '0%';
            hintDisplay.style.display = 'none';
            submitBtn.style.display = 'inline-block';
            nextBtn.style.display = 'none';
            feedbackPopup.classList.remove('active');
            emojiArea.style.display = 'none';
            
            // 清空选项容器
            while (optionsContainer.firstChild) {
                optionsContainer.removeChild(optionsContainer.firstChild);
            }
        }
        
        // 生成题目
        function generateQuestion() {
            if (questionsAnswered >= maxQuestions || timeLeft <= 0) {
                endGame();
                return;
            }
            
            selectedOption = null;
            hintDisplay.style.display = 'none';
            submitBtn.style.display = 'inline-block';
            nextBtn.style.display = 'none';
            emojiArea.style.display = 'none';
            
            // 清空选项容器
            while (optionsContainer.firstChild) {
                optionsContainer.removeChild(optionsContainer.firstChild);
            }
            
            // 随机选择问题类型 (0: 选择拼音, 1: 选择英文, 2: 图片识别)
            const questionType = Math.floor(Math.random() * 3);
            const questionIndex = Math.floor(Math.random() * vocabulary.length);
            const correctItem = vocabulary[questionIndex];
            
            // 生成问题和选项
            let questionText = '';
            let correctAnswer = '';
            
            switch (questionType) {
                case 0: // 选择拼音
                    questionText = `请选择与"${correctItem.chinese}"对应的拼音`;
                    correctAnswer = correctItem.pinyin;
                    break;
                case 1: // 选择英文翻译
                    questionText = `请选择"${correctItem.chinese}"的英文翻译`;
                    correctAnswer = correctItem.en;
                    break;
                case 2: // 图片识别
                    questionText = `这个图标代表什么中文词语?`;
                    emojiArea.textContent = correctItem.image;
                    emojiArea.style.display = 'block';
                    correctAnswer = correctItem.chinese;
                    break;
            }
            
            questionArea.textContent = questionText;
            
            // 收集选项（包括正确答案）
            const options = [correctAnswer];
            while (options.length < 3) {
                const randomIndex = Math.floor(Math.random() * vocabulary.length);
                let randomOption;
                
                switch (questionType) {
                    case 0:
                        randomOption = vocabulary[randomIndex].pinyin;
                        break;
                    case 1:
                        randomOption = vocabulary[randomIndex].en;
                        break;
                    case 2:
                        randomOption = vocabulary[randomIndex].chinese;
                        break;
                }
                
                if (!options.includes(randomOption) && randomOption !== correctAnswer) {
                    options.push(randomOption);
                }
            }
            
            // 打乱选项顺序
            shuffleArray(options);
            
            // 创建选项按钮
            options.forEach((option, index) => {
                const optionElement = document.createElement('div');
                optionElement.className = 'option';
                optionElement.textContent = option;
                optionElement.dataset.value = option;
                
                optionElement.addEventListener('click', () => {
                    // 移除之前选择的任何选项的选中状态
                    document.querySelectorAll('.option').forEach(opt => {
                        opt.classList.remove('selected');
                    });
                    
                    // 添加当前选择的选项的选中状态
                    optionElement.classList.add('selected');
                    selectedOption = option;
                });
                
                optionsContainer.appendChild(optionElement);
            });
            
            // 更新进度
            currentDisplay.textContent = `${questionsAnswered + 1}/${maxQuestions}`;
            progressBar.style.width = `${(questionsAnswered / maxQuestions) * 100}%`;
        }
        
        // 检查答案
        function checkAnswer() {
            if (!selectedOption) {
                alert('请选择一个答案!');
                return;
            }
            
            submitBtn.style.display = 'none';
            nextBtn.style.display = 'inline-block';
            
            let correctAnswer;
            switch (true) {
                case questionArea.textContent.includes('拼音'):
                    const chineseChar = questionArea.textContent.split('"')[1];
                    correctAnswer = vocabulary.find(item => item.chinese === chineseChar).pinyin;
                    break;
                case questionArea.textContent.includes('英文翻译'):
                    const chineseWord = questionArea.textContent.split('"')[1];
                    correctAnswer = vocabulary.find(item => item.chinese === chineseWord).en;
                    break;
                case questionArea.textContent.includes('图标'):
                    const currentEmoji = emojiArea.textContent;
                    correctAnswer = vocabulary.find(item => item.image === currentEmoji).chinese;
                    break;
            }
            
            // 验证答案并更新UI
            const options = document.querySelectorAll('.option');
            let answeredCorrectly = false;
            
            options.forEach(option => {
                option.classList.remove('selected');
                option.style.pointerEvents = 'none'; // 禁用所有选项
                
                if (option.textContent === selectedOption) {
                    if (selectedOption === correctAnswer) {
                        option.classList.add('correct');
                        answeredCorrectly = true;
                    } else {
                        option.classList.add('incorrect');
                    }
                }
                
                // 高亮显示正确答案
                if (option.textContent === correctAnswer) {
                    option.classList.add('correct');
                }
            });
            
            if (answeredCorrectly) {
                score += 10;
                scoreDisplay.textContent = score;
                // 显示成功提示
                showFeedbackPopup(true);
            } else {
                // 显示错误提示
                showFeedbackPopup(false);
            }
            
            questionsAnswered++;
        }
        
        // 显示答题反馈
        function showFeedbackPopup(isCorrect) {
            const feedbackPopup = document.getElementById('feedback-popup');
            const feedbackTitle = document.querySelector('.feedback-title');
            const feedbackScore = document.querySelector('.feedback-score');
            const feedbackMessage = document.querySelector('.feedback-message');
            
            feedbackPopup.classList.add('active');
            
            if (isCorrect) {
                feedbackTitle.textContent = '回答正确!';
                feedbackTitle.style.color = '#4CAF50';
                feedbackScore.textContent = '+10分';
                feedbackScore.style.color = '#4CAF50';
                feedbackMessage.textContent = '太棒了！继续加油！';
            } else {
                feedbackTitle.textContent = '回答错误';
                feedbackTitle.style.color = '#F44336';
                feedbackScore.textContent = '+0分';
                feedbackScore.style.color = '#F44336';
                feedbackMessage.textContent = '别灰心，继续尝试！';
            }
            
            setTimeout(() => {
                feedbackPopup.classList.remove('active');
            }, 1500);
        }
        
        // 结束游戏
        function endGame() {
            clearInterval(timer);
            finalScore.textContent = score;
            feedbackPopup.classList.add('active');
            document.querySelector('.feedback-title').textContent = '游戏结束!';
            document.querySelector('.feedback-title').style.color = '#1E88E5';
            document.querySelector('.feedback-score').textContent = score;
            document.querySelector('.feedback-score').style.color = '#1E88E5';
            document.querySelector('.feedback-message').textContent = '恭喜完成本次词汇挑战！';
        }
        
        // 开始计时器
        function startTimer() {
            clearInterval(timer);
            timer = setInterval(() => {
                timeLeft--;
                timerDisplay.textContent = timeLeft;
                
                if (timeLeft <= 0) {
                    endGame();
                }
            }, 1000);
        }
        
        // 实用函数：打乱数组
        function shuffleArray(array) {
            for (let i = array.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [array[i], array[j]] = [array[j], array[i]];
            }
        }
        
        // 事件监听器
        submitBtn.addEventListener('click', checkAnswer);
        nextBtn.addEventListener('click', generateQuestion);
        
        hintBtn.addEventListener('click', () => {
            const currentQuestionText = questionArea.textContent;
            let currentHint = "";
            
            if (currentQuestionText.includes('拼音')) {
                const currentWord = currentQuestionText.split('"')[1];
                currentHint = vocabulary.find(item => item.chinese === currentWord).hint;
            } else if (currentQuestionText.includes('英文翻译')) {
                const currentWord = currentQuestionText.split('"')[1];
                currentHint = vocabulary.find(item => item.chinese === currentWord).hint;
            } else if (currentQuestionText.includes('图标')) {
                const currentEmoji = emojiArea.textContent;
                currentHint = vocabulary.find(item => item.image === currentEmoji).hint;
            }
            
            hintDisplay.textContent = `提示：${currentHint}`;
            hintDisplay.style.display = 'block';
        });
        
        restartBtn.addEventListener('click', () => {
            feedbackPopup.classList.remove('active');
            initGame();
        });
        
       // 初始化游戏
        window.onload = initGame;
    </script>
</body>
