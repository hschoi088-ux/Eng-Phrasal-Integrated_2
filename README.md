<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>스피드 영어 퀴즈 (Week 2 최종 완성)</title>
    <style>
        body {
            font-family: 'Pretendard', -apple-system, BlinkMacSystemFont, system-ui, Roboto, sans-serif;
            background-color: #f0f4f8;
            color: #333;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
        }
        .container {
            background: white;
            padding: 30px;
            border-radius: 16px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.08);
            max-width: 650px;
            width: 100%;
        }
        h1 { text-align: center; margin-bottom: 20px; color: #1e293b; font-size: 26px; }
        
        /* Week 탭 스타일 */
        .week-tabs {
            display: flex;
            gap: 10px;
            margin-bottom: 25px;
            border-bottom: 2px solid #e2e8f0;
            padding-bottom: 10px;
            justify-content: center;
        }
        .week-tab {
            padding: 10px 20px;
            border: none;
            background: none;
            font-size: 16px;
            font-weight: bold;
            color: #94a3b8;
            cursor: pointer;
            border-radius: 8px;
            transition: all 0.2s;
        }
        .week-tab.active {
            background-color: #e0f2fe;
            color: #0284c7;
        }
        .week-tab:hover:not(.active) { background-color: #f1f5f9; }

        /* 카테고리 영역 */
        .category-section {
            margin-bottom: 25px;
            background: #f8fafc;
            padding: 20px;
            border-radius: 12px;
            border: 1px solid #e2e8f0;
        }
        .category-title {
            font-size: 18px; font-weight: bold; color: #334155;
            margin-top: 0; margin-bottom: 15px; display: flex; align-items: center; gap: 8px;
        }
        
        /* 순한맛 / 매운맛 버튼 */
        .flavor-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 15px; }
        @media (max-width: 480px) { .flavor-grid { grid-template-columns: 1fr; } }
        .flavor-btn {
            background: white; border: 2px solid #cbd5e1; border-radius: 10px;
            padding: 15px; text-align: left; cursor: pointer; transition: all 0.2s ease;
        }
        .flavor-btn:hover { border-color: #3b82f6; box-shadow: 0 4px 12px rgba(59, 130, 246, 0.15); transform: translateY(-2px); }
        .flavor-title { font-size: 16px; font-weight: bold; display: block; margin-bottom: 6px; }
        .flavor-desc { font-size: 13px; color: #64748b; line-height: 1.4; word-break: keep-all; }
        .t-easy { color: #10b981; } .t-hard { color: #ef4444; }
        .hidden { display: none !important; }
        
        /* 퀴즈 영역 스타일 */
        #quiz-area { display: flex; flex-direction: column; gap: 20px; }
        #question-counter { font-size: 15px; color: #64748b; font-weight: bold; text-align: center;}
        
        .question-box { 
            font-size: 22px; font-weight: bold; word-break: keep-all; line-height: 1.5; 
            background: #f8fafc; padding: 30px 20px; border-radius: 12px;
            border: 2px dashed #cbd5e1; cursor: pointer; transition: background 0.2s; text-align: center;
        }
        .question-box:hover { background: #e2e8f0; }
        .question-box::after { content: "(클릭하여 정답 확인)"; font-size: 13px; color: #94a3b8; display: block; margin-top: 10px; font-weight: normal; }

        .answer-box { 
            background: #ecfdf5; padding: 25px 20px; border-radius: 12px; border: 1px solid #a7f3d0; text-align: left;
        }
        .en-text { font-size: 22px; color: #059669; font-weight: bold; margin-bottom: 15px; line-height: 1.4; word-break: keep-all;}
        .meta-info { font-size: 14px; color: #475569; margin-bottom: 5px; background: #fff; display: inline-block; padding: 4px 10px; border-radius: 20px; border: 1px solid #e2e8f0; }
        .meaning-info { font-size: 15px; color: #b45309; margin-bottom: 15px; background: #fef3c7; padding: 8px 12px; border-radius: 8px; font-weight: 500;}
        
        .controls { display: flex; justify-content: space-between; align-items: center; margin-top: 10px; flex-wrap: wrap; gap: 10px;}
        .left-controls { display: flex; gap: 10px; }
        
        /* 버튼 스타일 */
        .btn { padding: 10px 20px; border: none; border-radius: 8px; cursor: pointer; font-weight: bold; font-size: 15px; transition: 0.2s;}
        .btn-tts { background-color: #8b5cf6; color: white; display: flex; align-items: center; gap: 5px; }
        .btn-tts:hover { background-color: #7c3aed; }
        .btn-star { background-color: #e2e8f0; color: #475569; }
        .btn-star.active { background-color: #fef08a; color: #ca8a04; border: 1px solid #fde047; }
        .btn-next { background-color: #10b981; color: white; }
        .btn-next:hover { background-color: #059669; }
        .btn-finish { background-color: #3b82f6; color: white; width: 100%; padding: 15px; font-size: 16px; margin-top: 10px; }
        .btn-finish:hover { background-color: #2563eb; }
        .btn-home { background-color: #64748b; color: white; width: 100%; padding: 15px; font-size: 16px; margin-top: 20px;}
        .btn-home:hover { background-color: #475569; }

        /* 복습(결과) 영역 스타일 */
        #review-area { display: flex; flex-direction: column; gap: 15px; }
        .review-card { background: #fff; border: 1px solid #e2e8f0; border-radius: 10px; padding: 15px; box-shadow: 0 2px 5px rgba(0,0,0,0.05); text-align: left;}
        .review-ko { font-size: 16px; font-weight: bold; color: #334155; margin-bottom: 8px; }
        .review-en { font-size: 18px; font-weight: bold; color: #059669; margin-bottom: 8px; }
        .review-empty { text-align: center; padding: 40px 20px; font-size: 18px; color: #10b981; font-weight: bold; background: #ecfdf5; border-radius: 12px;}
    </style>
</head>
<body>

<div class="container">
    <h1 id="main-title">🚀 스피드 영어 퀴즈</h1>
    
    <div id="mode-selection">
        <div class="week-tabs">
            <button class="week-tab active" onclick="setWeek(1, this)">Week 1</button>
            <button class="week-tab" onclick="setWeek(2, this)">Week 2</button>
            <button class="week-tab" onclick="setWeek('all', this)">Week 1~2 누적</button>
        </div>

        <div class="category-section">
            <h3 class="category-title">🧩 구동사 (Phrasal Verbs)</h3>
            <div class="flavor-grid">
                <button class="flavor-btn" onclick="startQuiz('phrasal-easy')">
                    <span class="flavor-title t-easy">🟢 순한맛</span>
                    <span class="flavor-desc">구동사 의미별로 짧고 쉬운 문장들이 선별해서 담겨있음</span>
                </button>
                <button class="flavor-btn" onclick="startQuiz('phrasal-hard')">
                    <span class="flavor-title t-hard">🔴 매운맛</span>
                    <span class="flavor-desc">전체 예문에서 선별됨</span>
                </button>
            </div>
        </div>

        <div class="category-section">
            <h3 class="category-title">🗣️ 영어회화 (Conversation)</h3>
            <div class="flavor-grid">
                <button class="flavor-btn" onclick="startQuiz('conv-easy')">
                    <span class="flavor-title t-easy">💬 순한맛</span>
                    <span class="flavor-desc">'대표문장'과 '교재1'만 담고 있음</span>
                </button>
                <button class="flavor-btn" onclick="startQuiz('conv-hard')">
                    <span class="flavor-title t-hard">🔥 매운맛</span>
                    <span class="flavor-desc">전체 예문에서 선별됨</span>
                </button>
            </div>
        </div>
    </div>

    <div id="quiz-area" class="hidden">
        <div id="question-counter">문제 1 / 7</div>
        
        <div class="question-box" id="ko-box" onclick="showAnswer()">
            <span id="ko-text">한국어 문장이 여기에 표시됩니다.</span>
        </div>

        <div class="answer-box hidden" id="answer-section">
            <div class="meta-info" id="source-info">출처: Day001 교재1</div>
            <div class="en-text" id="en-text">English Answer</div>
            <div class="meaning-info hidden" id="meaning-info">의미: </div>
            
            <div class="controls">
                <div class="left-controls">
                    <button class="btn btn-tts" onclick="playTTS()">🔊 듣기</button>
                    <button class="btn btn-star" id="btn-star" onclick="toggleStar()">⭐ 어려워요</button>
                </div>
                <button class="btn btn-next" id="btn-next" onclick="nextQuestion()">다음 문제 ➡</button>
            </div>
        </div>

        <button class="btn btn-finish hidden" id="btn-finish" onclick="showReview()">결과 보기 (오답 노트) 📝</button>
    </div>

    <div id="review-area" class="hidden">
        <h2 style="text-align: center; color: #1e293b; margin-top:0;">⭐ 나의 오답 노트</h2>
        <p style="text-align: center; color: #64748b; font-size: 14px; margin-top:-10px;">어려웠던 문장들을 다시 확인해 보세요!</p>
        
        <div id="review-list"></div>

        <button class="btn btn-home" onclick="resetToHome()">🏠 처음으로 돌아가기</button>
    </div>
</div>

<script>
    let currentWeekFilter = 1;

    function setWeek(week, btn) {
        currentWeekFilter = week;
        document.querySelectorAll('.week-tab').forEach(el => el.classList.remove('active'));
        btn.classList.add('active');
    }

    // 📌 모든 구동사 의미 매핑
    const meanings = {
        "add up_1": "add up: 1. (수 등을) 하나하나 더하다",
        "add up_2": "add up: 2. (점진적으로 쌓여) 합계가 결국 ~가 되다",
        "add up_3": "add up: 3. (주로 부정문에서) 앞뒤가 맞다, 논리적으로 말이 되다",
        "blow away_1": "blow away: 1. (바람 등이 사물을) 멀리 날려 보내다",
        "blow away_2": "blow away: 2. (사람을) 대단히 놀라게 하거나 깊은 감명을 주다",
        "break down_1": "break down: 1. (기계나 차량이) 고장이 나다",
        "break down_2": "break down: 2. (협상이나 시스템 등이) 결렬되다, 실패로 끝나다",
        "break down_3": "break down: 3. (슬픔 등을 못 참고) 울며 무너지다, 자제력을 잃다",
        "break down_4": "break down: 4. (이해를 돕기 위해) 세부 사항을 조목조목 나누어 설명하다",
        "break up_1": "break up: 1. (연인 등이 관계를 끝내고) 헤어지다",
        "break up_2": "break up: 2. (여러 조각이나 작은 그룹으로) 분해하다, 분리하다",
        "break up_3": "break up: 3. (전화 신호 등이) 잡음과 함께 뚝뚝 끊기다",
        "brush up on_1": "brush up on: 1. (오래전 배운 지식이나 기술을) 다시 복습하다, 다듬다",
        
        "care for_1": "care for: 1. ~을 좋아하다 (주로 부정문/의문문에서 쓰임)",
        "care for_2": "care for: 2. ~을 보살피다, 돌보다",
        "catch on_1": "catch on: 1. (패션, 트렌드 등이) 유행하다, 인기를 끌다",
        "catch on_2": "catch on: 2. (상황, 농담, 힌트 등을) 눈치채다, 파악하다",
        "catch up on_1": "catch up on: 1. (밀린 일, 잠 등을) 보충하다, 만회하다",
        "catch up on_2": "catch up on: 2. (밀린 소식, 정보 등을) 듣다, 알아내다",
        "check on_1": "check on / check in on: 1. (상태, 안부, 건강 등을) 확인하다",
        "check out_1": "check out: 1. (흥미로운 곳, 물건 등을) 살펴보다, 확인하다",
        "check out_2": "check out: 2. (지루함 등으로) 집중력이 흐트러져 딴생각을 하다",
        "check out_3": "check out: 3. (관계 등에서) 이미 마음이 떠나 신경을 끈 상태가 되다",
        "check out_4": "check out: 4. (정보나 사실의) 진위 여부를 대조하여 검증하다",
        
        // 새로 추가된 Week 2 순한맛 의미들
        "come across_1": "come across: (물건이나 사람을) 우연히 발견하다, 뜻밖에 마주치다",
        "come along_1": "come along: (제안을 받고 상대와) 동행하여 따라가다",
        "come along_2": "come along: (일이 계획대로) 순조롭게 진행되어 가다",
        "come along_3": "come along: (필요한 기회나 사람이) 예상치 못하게 나타나다",
        "come around_1": "come around: (기절 상태에서) 다시 의식이 돌아오다",
        "come around_2": "come around: (계절이나 기념일 등이) 때가 되어 다시 돌아오다",
        "come around_3": "come around: (의견 차이가 있다가) 결국 상대의 입장에 동의하거나 마음을 바꾸다",
        "come around_4": "come around: (누군가 있는) 특정한 장소에 방문하다, 오다",
        "come off_1": "come off: (붙어 있던 라벨 등이) 자연스럽게 떨어지거나 벗겨지다",
        "fall off_1": "fall off: (높은 곳이나 가장자리에서) 아래로 추락하다",
        "break off_1": "break off: (충격 등으로 부서지거나 꺾여서) 떨어져 나가다",
        "come off as_1": "come off as: (남들에게) 특정한 태도나 성격처럼 비치다",
        "come across as_1": "come across as: (의도와 상관없이) 어떠한 인상을 주다",
        "come up with_1": "come up with: (아이디어 등을) 자연스럽게 생각해내다",
        "come up with_2": "come up with: (급한 상황에서) 아쉬운 대로 해결책이나 돈을 마련해내다",
        "cut off_1": "cut off: (상대방의 말을) 중간에 가로막아 끊다",
        "cut off_2": "cut off: (전기, 물 공급이나 인터넷 접속을) 완전히 차단하다",
        "cut off_3": "cut off: (도로 주행 중) 다른 차 앞으로 위험하게 끼어들다",
        "cut off_4": "cut off: (외부와 담을 쌓고) 자신을 고립시키거나 단절시키다",
        "cut off_5": "cut off: (화면이나 지면 끝에서) 내용이 잘려서 보이다",
        "figure out_1": "figure out: (어려운 수학 문제나 퀴즈 등을) 논리적으로 풀다",
        "figure out_2": "figure out: (복잡한 기계나 시스템의) 작동법을 알아내다"
    };

    // 1. 구동사 순한맛 (Week 1 + Week 2 통합)
    const phrasalEasy = [
        // Week 1
        { week: 1, ko: "자, 이 숫자들을 더해 보자.", en: "Let’s add up these numbers now.", source: "Day 001 순한맛", meaning: meanings["add up_1"] },
        { week: 1, ko: "별것 아니게 보일 수 있어도, 하루 10분의 연습도 쌓이면 정말 큽니다.", en: "It might not seem like much, but 10 minutes of practice every day really adds up.", source: "Day 001 순한맛", meaning: meanings["add up_2"] },
        { week: 1, ko: "뭔가 앞뒤가 안 맞잖아.", en: "Something doesn’t add up.", source: "Day 001 순한맛", meaning: meanings["add up_3"] },
        { week: 1, ko: "모자가 강풍에 날아가 버렸다.", en: "My hat blew away in the strong wind.", source: "Day 002 순한맛", meaning: meanings["blow away_1"] },
        { week: 1, ko: "정말 놀랐어요!", en: "You really blew me away!", source: "Day 002 순한맛", meaning: meanings["blow away_2"] },
        { week: 1, ko: "제 차가 고속 도로에서 고장이 났습니다.", en: "My car broke down on the highway.", source: "Day 003 순한맛", meaning: meanings["break down_1"] },
        { week: 1, ko: "근무시간 관련 협상이 결렬된 것은 불가피했습니다.", en: "It was inevitable that the negotiation over working hours broke down.", source: "Day 003 순한맛", meaning: meanings["break down_2"] },
        { week: 1, ko: "의사 선생님이 제가 다시는 프로 선수로 뛸 수 없다고 했을 때 저는 무너졌습니다.", en: "When the doctor said I could never play professionally again, I broke down.", source: "Day 003 순한맛", meaning: meanings["break down_3"] },
        { week: 1, ko: "스케줄을 세부적으로 말씀드릴게요.", en: "Let me break down the schedule.", source: "Day 003 순한맛", meaning: meanings["break down_4"] },
        { week: 1, ko: "사실 Susie가 헤어지자고 해서 헤어진 거야.", en: "She broke up with me, actually.", source: "Day 004 순한맛", meaning: meanings["break up_1"] },
        { week: 1, ko: "3명씩 조를 나누어라.", en: "I need you to break up into groups of three.", source: "Day 004 순한맛", meaning: meanings["break up_2"] },
        { week: 1, ko: "네 말이 끊겨서 들려.", en: "You are breaking up.", source: "Day 004 순한맛", meaning: meanings["break up_3"] },
        { week: 1, ko: "지난 학기에 배운 내용을 다시 한번 복습해 보겠습니다.", en: "I’d like us to brush up on what we learned last semester.", source: "Day 005 순한맛", meaning: meanings["brush up on_1"] },
        
        // Week 2
        { week: 2, ko: "이 시계 자세히 봐봐.", en: "Check out this watch.", source: "Day 010 순한맛", meaning: meanings["check out_4"] },
        { week: 2, ko: "불과 몇 분 만에 수업 듣는 학생 대부분이 딴생각을 하기 시작했다.", en: "It only took a few minutes before most of the class had mentally checked out.", source: "Day 010 순한맛", meaning: meanings["check out_2"] },
        { week: 2, ko: "Samantha는 저랑 헤어지기 한참 전부터 이미 마음이 떠났다고 하더라고요.", en: "Samantha said she had already checked out of the relationship long before she broke up with me.", source: "Day 010 순한맛", meaning: meanings["check out_3"] },
        { week: 2, ko: "걸어서 집에 오다가 자두나무를 봤어요.", en: "I came across a plum tree while walking home.", source: "Day 011 순한맛", meaning: meanings["come across_1"] },
        { week: 2, ko: "혹시 파티에 따라가도 될까?", en: "Do you mind if I come along with you to the party?", source: "Day 012 순한맛", meaning: meanings["come along_1"] },
        { week: 2, ko: "공부는 잘되어 가니?", en: "How are your studies coming along?", source: "Day 012 순한맛", meaning: meanings["come along_2"] },
        { week: 2, ko: "당신이 나타날 때까지 나는 방황했어.", en: "I was lost until you came along.", source: "Day 012 순한맛", meaning: meanings["come along_3"] },
        { week: 2, ko: "의식이 돌아왔을 때 괜찮았는데, 그 후 20분 정도 지나니 마취가 완전히 풀리더군요.", en: "I felt okay when I came around, but the anesthesia fully wore off about 20 minutes after that.", source: "Day 013 순한맛", meaning: meanings["come around_1"] },
        { week: 2, ko: "여름이 될 때마다 다시 태어난 기분이에요.", en: "Every time summer comes around, I feel like a brand-new person.", source: "Day 013 순한맛", meaning: meanings["come around_2"] },
        { week: 2, ko: "이번에도 틀림없이 풀릴 거야.", en: "I’m sure he’ll come around this time, too.", source: "Day 013 순한맛", meaning: meanings["come around_3"] },
        { week: 2, ko: "먼 친척들이 올 때면 집이 좀 북적이고 분주한 느낌이 든답니다.", en: "Whenever our distant relatives come around, the house always feels a bit more crowded and hectic.", source: "Day 013 순한맛", meaning: meanings["come around_4"] },
        { week: 2, ko: "그 병에 붙은 라벨은 물에 들어가면 쉽게 벗겨진다.", en: "The labels on the bottles come off so easily in water.", source: "Day 014 순한맛", meaning: meanings["come off_1"] },
        { week: 2, ko: "아들에게 절벽 가장자리로 너무 가까이 가지 말라고 경고했다, 혹시 떨어질 수도 있어서.", en: "I warned my son not to get too close to the cliff’s edge, or else he might fall off.", source: "Day 014 순한맛", meaning: meanings["fall off_1"] },
        { week: 2, ko: "충돌 사고로 우측 사이드 미러가 부서져 떨어져 나갔다.", en: "The right side-view mirror broke off the car in the collision.", source: "Day 014 순한맛", meaning: meanings["break off_1"] },
        { week: 2, ko: "거친 외모와는 달리 그는 실제로는 온화하고 따뜻한 사람처럼 보였다.", en: "Despite his rough looks, he actually came off as gentle and caring.", source: "Day 015 순한맛", meaning: meanings["come off as_1"] },
        { week: 2, ko: "면접에서는 자신감 있고 똑똑하다는 인상을 주는 것이 중요하다.", en: "In interviews, it’s important to come across as confident and knowledgeable.", source: "Day 015 순한맛", meaning: meanings["come across as_1"] },
        { week: 2, ko: "나는 항상 샤워를 할 때 좋은 생각이 떠온다.", en: "I always come up with good ideas in the shower.", source: "Day 016 순한맛", meaning: meanings["come up with_1"] },
        { week: 2, ko: "이것이 생각나는 최선의 번역입니다.", en: "This is the best translation I could come up with.", source: "Day 016 순한맛", meaning: meanings["come up with_2"] },
        { week: 2, ko: "내가 이야기를 하고 있는데 사람들이 말을 끊으면 정말 짜증난다.", en: "My pet peeve is when people cut me off when I’m in the middle of a story.", source: "Day 017 순한맛", meaning: meanings["cut off_1"] },
        { week: 2, ko: "나 인스타 접속이 막혔어.", en: "I’ve just been cut off, is all.", source: "Day 017 순한맛", meaning: meanings["cut off_2"] },
        { week: 2, ko: "제 남편은 도로에서 차들이 끼어드는 걸 너무 싫어합니다.", en: "My husband hates when people cut him off in the middle of traffic.", source: "Day 017 순한맛", meaning: meanings["cut off_3"] },
        { week: 2, ko: "최근에 왜 연락도 없고 담을 쌓고 지내는 거야?", en: "Why have you been cutting yourself off from us lately?", source: "Day 017 순한맛", meaning: meanings["cut off_4"] },
        { week: 2, ko: "페이지 설정이 잘못된 듯하네, 각 줄마다 마지막 단어가 다 잘렸어.", en: "It seems like the page formatting was off, because the last word in every line got cut off.", source: "Day 017 순한맛", meaning: meanings["cut off_5"] },
        { week: 2, ko: "드디어 루빅큐브 맞추는 방법 알아냈다.", en: "I finally figured out how to solve the Rubik’s Cube.", source: "Day 018 순한맛", meaning: meanings["figure out_1"] },
        { week: 2, ko: "새로 들인 복사기 작동법을 도무지 모르겠어.", en: "I can’t seem to figure out how to work this new photocopier.", source: "Day 018 순한맛", meaning: meanings["figure out_2"] }
    ];

    // 2. 구동사 매운맛 (Week 1 + Week 2)
    const phrasalHard = [
        // Week 1 
        { week: 1, ko: "별것 아니게 보일 수 있어도, 하루 10분의 연습도 쌓이면 정말 큽니다.", en: "It might not seem like much, but 10 minutes of practice every day really adds up.", source: "Day001 교재1", meaning: meanings["add up_2"]},
        { week: 1, ko: "어제 집에 있었다고 했는데, 내 친구가 당신을 술집에서 봤다고 했어. 뭔가 앞뒤가 안 맞잖아.", en: "You told me you were at home, but my friend mentioned seeing you at a bar. Something doesn’t add up.", source: "Day001 교재1", meaning: meanings["add up_3"]},
        { week: 1, ko: "자, 이 숫자들을 더해 보자. 5 더하기 3은 뭘까?", en: "Let’s add up these numbers now. What’s five plus three?", source: "Day001 교재1", meaning: meanings["add up_1"]},
        { week: 1, ko: "월 10만 원도 쌓이면 4년 후에 거의 5백만 원이 된다.", en: "Just 100,000 won a month will add up to almost 5 million won in four years.", source: "Day001 교재1", meaning: meanings["add up_2"]},
        { week: 1, ko: "그의 이야기에는 앞뒤가 맞지 않는 것이 있어요. 그가 거짓말을 하고 있는 게 틀림없어요.", en: "There’s something about his story that doesn’t add up. He must be telling a lie.", source: "Day001 교재1", meaning: meanings["add up_3"]},
        { week: 1, ko: "한 달에 커피값으로 백 달러 정도를 지출해. 나의 길티 플레저거든.", en: "I spend around a hundred bucks a month on coffee. It’s my guilty pleasure.", source: "Day001 교재2", meaning: meanings["add up_2"]},
        { week: 1, ko: "한 달에 백 달러도 5년이면 6천 달러야. 집에서 만들어 먹는 게 어때?", en: "A hundred bucks a month will add up to $6,000 in five years. Why don’t you make coffee at home?", source: "Day001 교재2", meaning: meanings["add up_2"]},
        { week: 1, ko: "지난 분기 매출 수치를 합해서 목표치와 비교해 주시겠어요?", en: "Can you add up the sales figures from last quarter and compare them to our targets?", source: "Day001 교재2", meaning: meanings["add up_1"]},
        { week: 1, ko: "물론입니다. 계산해서 오늘 오후까지 세부 보고서를 준비해 두겠습니다.", en: "Sure thing. I’ll do the math and have a detailed report ready by this afternoon.", source: "Day001 교재2", meaning: meanings["add up_1"]},
        { week: 1, ko: "Brian은 너무 완벽해. 우리 다음 달에 오프라인에서 만날 거야.", en: "Brian is so perfect. We’re going to meet offline next month.", source: "Day001 교재2", meaning: meanings["add up_3"]},
        { week: 1, ko: "그러니까 의사이자 파일럿이고, 외모는 모델 같은데, 아직 사진을 안 보여 줬다? 이건 말이 안 돼.", en: "So he’s a doctor, a pilot, and he looks like a model, yet he hasn’t shown you a photo? This doesn’t add up, Gina.", source: "Day001 교재2", meaning: meanings["add up_3"]},
        { week: 1, ko: "이게 쌓이면 정말 큽니다.", en: "it really adds up.", source: "Day001 교재3", meaning: meanings["add up_2"]},
        { week: 1, ko: "어젯밤에 눈이 많이 와서 미끄러우면 어쩌나 했지만, 바람이 워낙 강해서 눈이 다 날아가 버렸더군.", en: "I was worried that sidewalks would be slippery, but the wind was so strong that it blew the snow away.", source: "Day002 교재1", meaning: meanings["blow away_1"]},
        { week: 1, ko: "그냥 압축공기를 이용해서 먼지를 날려 버린답니다.", en: "I just use compressed air to blow the dust away.", source: "Day002 교재1", meaning: meanings["blow away_1"]},
        { week: 1, ko: "조심해! 자칫 날아갈 수도 있어!", en: "Be careful! You might get blown away!", source: "Day002 교재1", meaning: meanings["blow away_1"]},
        { week: 1, ko: "이번에 새로 나온 태블릿을 처음 봤을 때 매우 인상적이었어.", en: "When I first saw their new tablet, I was blown away.", source: "Day002 교재1", meaning: meanings["blow away_2"]},
        { week: 1, ko: "우와, 프레젠테이션 정말 유익했어요. 정말 놀랐어요!", en: "Wow, your presentation was so informative. You really blew me away!", source: "Day002 교재1", meaning: meanings["blow away_2"]},
        { week: 1, ko: "바람이 많이 부는 날씨는 불쾌하지만, 적어도 미세 먼지를 날려 버리긴 하지.", en: "Windy weather is unpleasant, but at least it blows away all the microdust.", source: "Day002 교재1", meaning: meanings["blow away_1"]},
        { week: 1, ko: "모자가 강풍에 날아가 버렸다.", en: "My hat blew away in the strong wind.", source: "Day002 교재1", meaning: meanings["blow away_1"]},
        { week: 1, ko: "제 동생이 영어를 너무 잘해서 정말 놀랐어요.", en: "I was really blown away by his English.", source: "Day002 교재1", meaning: meanings["blow away_2"]},
        { week: 1, ko: "바람이 너무 세서 자동차도 날아갔나 보더라고.", en: "Apparently, the wind was so strong that it even blew away cars.", source: "Day002 교재2", meaning: meanings["blow away_1"]},
        { week: 1, ko: "주연 배우의 연기가 정말 인상적이더라고.", en: "I was absolutely blown away by the main actor’s performance.", source: "Day002 교재2", meaning: meanings["blow away_2"]},
        { week: 1, ko: "김종국이 외국인들과 대화하는 거 봤는데 정말 대단하더라.", en: "I was completely blown away when I saw him chatting with foreigners.", source: "Day002 교재2", meaning: meanings["blow away_2"]},
        { week: 1, ko: "메뉴 볼 때마다 깜짝 놀라. 만 오천 원 이하가 없다니까.", en: "I’m blown away every time I look at a menu. You can’t eat for less than 15,000 won these days.", source: "Day002 교재3", meaning: meanings["blow away_2"]},
        { week: 1, ko: "몇 년을 매일 썼더니 컴퓨터가 결국 고장이 났다.", en: "The computer finally broke down after using it daily for years.", source: "Day003 교재1", meaning: meanings["break down_1"]},
        { week: 1, ko: "근무시간 관련 협상이 결렬된 것은 불가피했습니다.", en: "It was inevitable that the negotiation over working hours broke down.", source: "Day003 교재1", meaning: meanings["break down_2"]},
        { week: 1, ko: "의사 선생님이 제가 다시는 프로 선수로 뛸 수 없다고 했을 때 저는 무너졌습니다.", en: "When the doctor said I could never play professionally again, I broke down.", source: "Day003 교재1", meaning: meanings["break down_3"]},
        { week: 1, ko: "스케줄을 세부적으로 말씀드릴게요.", en: "Let me break down the schedule.", source: "Day003 교재1", meaning: meanings["break down_4"]},
        { week: 1, ko: "수치가 잘 이해되지 않네요. 미안한데 다시 한번 자세히 설명해 주시겠어요?", en: "Would you mind going back and breaking those down?", source: "Day003 교재1", meaning: meanings["break down_4"]},
        { week: 1, ko: "담배꽁초가 분해되는 데 18개월에서 10년이 걸리는 거 알았어?", en: "Did you know that cigarette butts take between 18 months and 10 years to break down?", source: "Day003 교재1", meaning: meanings["break down_2"]},
        { week: 1, ko: "제가 고속 도로에서 차가 고장 났습니다.", en: "My car broke down on the highway.", source: "Day003 교재1", meaning: meanings["break down_1"]},
        { week: 1, ko: "항상 돈 이야기가 나오면 이런 대화가 깨집니다.", en: "but those talks always break down once money comes up.", source: "Day003 교재1", meaning: meanings["break down_2"]},
        { week: 1, ko: "회사에서 계속 압박에 시달린 후에 결국 감정적으로 무너졌고 사무실에서 울었어요.", en: "After weeks of constant pressure at work, I finally broke down and cried in my office.", source: "Day003 교재1", meaning: meanings["break down_3"]},
        { week: 1, ko: "문장을 좀 더 잘 이해하기 위해서, 여러 단위로 나눠서 표현 하나하나를 살펴봤습니다.", en: "In order to understand the sentence better, I broke it down into parts and looked at the phrases one by one.", source: "Day003 교재1", meaning: meanings["break down_4"]},
        { week: 1, ko: "동물성 단백질은 분해되는 데 더 많은 에너지를 필요로 한다.", en: "Protein from meat requires more energy for your body to break down.", source: "Day003 교재1", meaning: meanings["break down_2"]},
        { week: 1, ko: "사용하던 컴퓨터가 결국 고장이 났거든요.", en: "My old computer finally broke down.", source: "Day003 교재2", meaning: meanings["break down_1"]},
        { week: 1, ko: "정신적으로 완전히 무너져서 아무 말도 안 나왔어.", en: "I just broke down completely and couldn’t even get words out.", source: "Day003 교재2", meaning: meanings["break down_3"]},
        { week: 1, ko: "아파트 청약 제도를 잘 모르겠습니다. 자세히 좀 설명해 주시겠어요?", en: "I still don’t understand how apartment lotteries work. Could you break it down for me?", source: "Day003 교재2", meaning: meanings["break down_4"]},
        { week: 1, ko: "버려진 후에 분해되는 데 정말 오래 걸려.", en: "They take forever to break down once they’re thrown away.", source: "Day003 교재3", meaning: meanings["break down_2"]},
        { week: 1, ko: "너 Susie랑 헤어졌다는 게 사실이야?", en: "Is it true you broke up with Susie?", source: "Day004 교재1", meaning: meanings["break up_1"]},
        { week: 1, ko: "사실 Susie가 헤어지자고 해서 헤어진 거야.", en: "She broke up with me, actually.", source: "Day004 교재1", meaning: meanings["break up_1"]},
        { week: 1, ko: "구름이 서서히 걷히고 해가 더 밝아졌다.", en: "The clouds gradually broke up and the sun got brighter.", source: "Day004 교재1", meaning: meanings["break up_2"]},
        { week: 1, ko: "닭고기를 한 입 크기로 찢은 다음, 샐러드에 넣어 섞으세요.", en: "First, use two forks to break up the chicken into bite-size pieces.", source: "Day004 교재1", meaning: meanings["break up_2"]},
        { week: 1, ko: "여기 (지하라서) 신호가 끊겨.", en: "My signal is breaking up down here.", source: "Day004 교재1", meaning: meanings["break up_3"]},
        { week: 1, ko: "사람들은 매일 같이 헤어지잖아. 너무 힘들게 받아들이지 마!", en: "People break up every day. Don’t take it so hard!", source: "Day004 교재1", meaning: meanings["break up_1"]},
        { week: 1, ko: "자, 얘들아. 3명씩 조를 나누어라.", en: "OK, class. I need you to break up into groups of three.", source: "Day004 교재1", meaning: meanings["break up_2"]},
        { week: 1, ko: "오노 요코 때문에 비틀즈가 해체됐다고 한다.", en: "My dad says Yoko Ono broke up The Beatles.", source: "Day004 교재1", meaning: meanings["break up_1"]},
        { week: 1, ko: "저는 하루 일과를 다양한 종류의 업무로 쪼갭니다.", en: "I like to break up my day with various kinds of tasks.", source: "Day004 교재1", meaning: meanings["break up_2"]},
        { week: 1, ko: "네 말이 끊겨서 들려.", en: "You are breaking up.", source: "Day004 교재1", meaning: meanings["break up_3"]},
        { week: 1, ko: "아무도 안 볼 텐데. 좀 더 짧은 클립으로 쪼개야 해.", en: "No one is gonna watch that. You need to break it up into smaller clips.", source: "Day004 교재2", meaning: meanings["break up_2"]},
        { week: 1, ko: "잠시만, 잘 안 들려. 신호가 계속 끊기네.", en: "Hold on, I can’t hear you. Your signal keeps breaking up.", source: "Day004 교재2", meaning: meanings["break up_3"]},
        { week: 1, ko: "그러니까 밴드가 해산하는 건 미나 잘못이야.", en: "so it’s her fault the band is breaking up.", source: "Day004 교재2", meaning: meanings["break up_1"]},
        { week: 1, ko: "하지만 급기야 둘 간의 관계가 흔들렸고 최근에 헤어지게 되었다.", en: "However, their relationship eventually got rocky, and they recently broke up.", source: "Day004 교재3", meaning: meanings["break up_1"]},
        { week: 1, ko: "“민호랑 헤어진 마당에 서울에 계속 있어야 할 이유가 없어.”", en: "She told me, “Now that Minho and I broke up, I don’t have any reason to stay in Seoul.”", source: "Day004 교재3", meaning: meanings["break up_1"]},
        
        // Week 2 
        { week: 2, ko: "다음 달에 파리로 여행 가는데, 프랑스어 복습 좀 해야겠어.", en: "I am travelling to Paris next month, so I think I need to brush up on my French.", source: "Day 005 교재1", meaning: meanings["brush up on_1"] },
        { week: 2, ko: "지난 수업에서 배운 내용을 복습해 보겠습니다.", en: "I’d like us to review what we learned last class.", source: "Day 005 교재1", meaning: meanings["brush up on_1"] },
        { week: 2, ko: "지난 학기에 배운 내용을 다시 한번 복습해 보겠습니다.", en: "I’d like us to brush up on what we learned last semester.", source: "Day 005 교재1", meaning: meanings["brush up on_1"] },
        { week: 2, ko: "나 케이크 만들어야 해. 빵 굽는 것을 연습하려면 요리책 읽어 봐야겠다.", en: "I’ve got to make a cake. I’m going to read a cookbook so that I can brush up on my baking skills.", source: "Day 005 교재1", meaning: meanings["brush up on_1"] },
        { week: 2, ko: "이 책은 ‘한 번 읽고 끝내는’ 책이 아닙니다. 잊어버릴 만하면 (이 책에 담긴) 구동사를 복습하셔야 실제 상황에서 자신 있게 쓸 수 있습니다.", en: "This book is not a “one-and-done” read. You should brush up on these phrasal verbs from time to time in order to feel confident using them in real-life situations.", source: "Day 005 교재1", meaning: meanings["brush up on_1"] },
        { week: 2, ko: "A: 은퇴 생활은 어떠신가요? 무료하신가요?", en: "A: So, how’s retired life treating you? Are you bored yet?", source: "Day 005 교재1", meaning: null },
        { week: 2, ko: "B: 전혀요. 한동안 안 치던 기타도 연습하고 있어요. 내가 기타 연주를 얼마나 좋아했는지를 잊고 있었어요.", en: "B: Not at all. I’ve been brushing up on my guitar playing. I forgot how much I loved playing.", source: "Day 005 교재1", meaning: meanings["brush up on_1"] },
        { week: 2, ko: "내일 경복궁 가는 거 정말 기대돼.", en: "I’m excited to visit Gyeongbokgung Palace tomorrow.", source: "Day 005 교재2", meaning: null },
        { week: 2, ko: "나도. 가기 전에 한국 역사 복습 좀 해야겠어. 그래야 경복궁에 가서 구경하게 될 것을 좀 더 잘 감상할 수 있을 테니.", en: "Me, too. I want to brush up on my Korean history before we go, so I can better appreciate what I’m seeing while we’re there.", source: "Day 005 교재2", meaning: meanings["brush up on_1"] },
        { week: 2, ko: "Janet, 내일 회의 때 제안서 발표하는 거죠?", en: "So, Janet, you’re going to present our proposal at the meeting tomorrow, right?", source: "Day 005 교재2", meaning: null },
        { week: 2, ko: "맞아요. 이번 주는 집에서 거울 보고 연습하면서 발표 스킬을 좀 가다듬고 있어요.", en: "Right. I’ve been brushing up on my presentation skills this week by practicing in the mirror at home.", source: "Day 005 교재2", meaning: meanings["brush up on_1"] },
        { week: 2, ko: "친구들한테 제가 만든 자장면이 정말 맛있다고 자랑을 했더니, 이번 주 토요일에 점심 먹으러 오겠대요. 그래서 요리 연습을 좀 해야 해요. (자장면) 안 만들어 본 지 10년도 넘었거든요.", en: "I was bragging about how good my homemade jajangmyeon is to my friends, and they invited themselves over for lunch this Saturday. I need to brush up on my cooking skills, because I haven’t made it in over 10 years.", source: "Day 005 교재2", meaning: meanings["brush up on_1"] },
        { week: 2, ko: "고등학교 때 만난 여자 친구와 10년을 사귀었는데 최근에 헤어졌다.", en: "After dating my high school girlfriend for 10 years, we recently broke up.", source: "Day 005 교재3", meaning: meanings["break up_1"] },
        { week: 2, ko: "“지도 있으세요? 당신의 눈에서 길을 잃어서요.”와 같은 (소개팅에서 쓸 수 있는) 작업 멘트를 연습해 보는 것이 내가 생각할 수 있는 전부였다.", en: "All I could think to do was brush up on some pickup lines like “Do you have a map? Because I just got lost in your eyes.” I hope it works.", source: "Day 005 교재3", meaning: meanings["brush up on_1"] },
        { week: 2, ko: "커피나 차 좀 드시겠어요?", en: "Would you care for some coffee or tea?", source: "Day 006 교재1", meaning: meanings["care for_1"] },
        { week: 2, ko: "죄송하지만 Jones 상무님은 아직 회의 중이시며, 마치려면 좀 걸릴 수 있습니다. 기다리시는 동안 마실 것 좀 가져다 드릴까요?", en: "I’m afraid Mr. Jones is still in a meeting, and it may take a while before he’s available. Would you care for something to drink while you wait?", source: "Day 006 교재1", meaning: meanings["care for_1"] },
        { week: 2, ko: "저는 고수를 별로 좋아하지 않습니다.", en: "I don’t really care for cilantro.", source: "Day 006 교재1", meaning: meanings["care for_1"] },
        { week: 2, ko: "우리 아빠 매운 음식 안 좋아하시는 거 알잖아.", en: "You know my dad doesn’t care much for spicy food.", source: "Day 006 교재1", meaning: meanings["care for_1"] },
        { week: 2, ko: "제가 휴가 가는 동안 우리 집 식물 좀 돌봐 주실 수 있으세요?", en: "Could you care for my plants while I’m on vacation?", source: "Day 006 교재1", meaning: meanings["care for_2"] },
        { week: 2, ko: "Sally, 너 정신 건강을 좀 챙겨야겠어.", en: "You need to care for your mental health, Sally.", source: "Day 006 교재1", meaning: meanings["care for_2"] },
        { week: 2, ko: "쿠션 드릴까요? 그 의자 조금 불편할 거예요.", en: "Would you care for a seat cushion? I know that chair can be a little uncomfortable.", source: "Day 006 교재1", meaning: meanings["care for_1"] },
        { week: 2, ko: "저녁 먹고 산책 가실래요?", en: "Would you care for a walk after dinner?", source: "Day 006 교재1", meaning: meanings["care for_1"] },
        { week: 2, ko: "괜찮아요. 전 단 것을 별로 안 좋아해서요.", en: "No thanks. I don’t care much for sweets.", source: "Day 006 교재1", meaning: meanings["care for_1"] },
        { week: 2, ko: "저는 무서운 영화를 별로 좋아하지 않아요.", en: "I don’t really care for scary movies.", source: "Day 006 교재1", meaning: meanings["care for_1"] },
        { week: 2, ko: "최근에 연로하신 부모님을 돌보기 시작했는데, 저를 키우느라 얼마나 많은 희생을 하셨는지 생각하니 정말 감사한 마음이 들었답니다.", en: "I recently started caring for my elderly parents, and it’s made me really grateful for how much they sacrificed to raise me.", source: "Day 006 교재1", meaning: meanings["care for_2"] },
        { week: 2, ko: "사실, 저 친구(화장실에 간 친구)는 해산물 별로 안 좋아해요.", en: "Actually, he doesn’t care for seafood.", source: "Day 006 교재2", meaning: meanings["care for_1"] },
        { week: 2, ko: "식물 기르는 거 보기보다 쉬워. 흙이 마르면 물만 주면 되거든.", en: "Caring for plants is easier than it looks, actually. All you have to do is water them when the soil is dry.", source: "Day 006 교재2", meaning: meanings["care for_2"] },
        { week: 2, ko: "게다가 Sam이랑 Frank는 남부 출신인데, 남부 사람들은 뉴요커를 별로 안 좋아해.", en: "Besides, Sam and Frank are from the South, and Southerners don’t care for New Yorkers...", source: "Day 006 교재2", meaning: meanings["care for_1"] },
        { week: 2, ko: "보살핌을 받는다는 느낌을 받을 수 없었습니다.", en: "I didn’t really feel cared for.", source: "Day 006 교재3", meaning: meanings["care for_2"] },
        { week: 2, ko: "헐렁한 셔츠와 바지가 다시 유행할 줄은 몰랐다.", en: "I didn’t expect baggy shirts and pants to catch on again.", source: "Day 007 교재1", meaning: meanings["catch on_1"] },
        { week: 2, ko: "최근 들어 (돈 문제로) 남편이랑 다투고 있다. 하지만 아이들이 우리 돈 문제를 눈치 못 채도록 노력하고 있다.", en: "My husband and I have been fighting recently, but I’m trying not to let my children catch on to our money problems.", source: "Day 007 교재1", meaning: meanings["catch on_2"] },
        { week: 2, ko: "우리 회사는 트렌드에 많이 뒤처져 있습니다. 재택근무의 수많은 장점을 아직 이해하지 못하고 있어요.", en: "My company is so behind the times. They still haven’t caught on to all the benefits that come with working from home.", source: "Day 007 교재1", meaning: meanings["catch on_2"] },
        { week: 2, ko: "스포츠 도박이 북미에서 선풍적인 인기를 끌고 있습니다.", en: "Sports betting is really catching on in North America.", source: "Day 007 교재1", meaning: meanings["catch on_1"] },
        { week: 2, ko: "Dave의 셔츠 뒷면에 ‘나는 바보다’라고 적혀 있었다. 그는 선생님이 그게 뭐냐고 물을 때까지 눈치를 못 챘다.", en: "Dave had “I’m an idiot” written on the back of his shirt. He didn’t catch on until the teacher asked him about it.", source: "Day 007 교재1", meaning: meanings["catch on_2"] },
        { week: 2, ko: "너희들 왜 계속 웃는 거야? 나는 너희들 유머가 전혀 이해가 안 되는데….", en: "Why do you guys keep laughing? I can never catch on to your jokes…", source: "Day 007 교재1", meaning: meanings["catch on_2"] },
        { week: 2, ko: "제 고객은 새로운 운동을 가르쳐 줄 때 이해하는 속도가 더딥니다.", en: "My client is slow to catch on when I teach him new exercises.", source: "Day 007 교재1", meaning: meanings["catch on_2"] },
        { week: 2, ko: "그랬더니? 정장 입고 수업에 온 거야, 아니면 그 전에 (장난으로 한 말이라는 걸) 눈치챈 거야?", en: "And what happened? Did he come to class dressed up, or did he catch on before that?", source: "Day 007 교재2", meaning: meanings["catch on_2"] },
        { week: 2, ko: "내 남편은 눈치가 너무 없어. 생일 선물 갖고 싶은 게 있으면 남편 쇼핑 리스트에 적어 줘야 한다니까.", en: "My husband doesn’t catch on to my hints.", source: "Day 007 교재2", meaning: meanings["catch on_2"] },
        { week: 2, ko: "보통 신입 서빙 직원을 교육하는 데 최대 일주일 정도 걸리는데, 당신은 상당히 빨리 익히는군요.", en: "Normally, it can take up to a week to train new servers, but you’re catching on pretty quick.", source: "Day 007 교재2", meaning: meanings["catch on_2"] },
        { week: 2, ko: "2~3년 전부터 전기차가 인기를 끌기 시작했습니다.", en: "Electric vehicles started to catch on a couple of years ago.", source: "Day 007 교재3", meaning: meanings["catch on_1"] },
        { week: 2, ko: "주말을 이용해 밀린 집안일을 해야겠다. 빨래가 계속 쌓이고 있다.", en: "I’ll use the weekend to catch up on household chores; the laundry has been piling up.", source: "Day 008 교재1", meaning: meanings["catch up on_1"] },
        { week: 2, ko: "아직이요. 점심 식사 후에 처리할게요. 오전에 일이 너무 많네요.", en: "Not yet. I’ll catch up on them after lunch; I’ve been swamped all morning.", source: "Day 008 교재1", meaning: meanings["catch up on_1"] },
        { week: 2, ko: "다음 주 언제 만나서 점심이나 먹으면 좋은데. 그동안 밀린 이야기가 정말 많잖아.", en: "Maybe we could meet up for lunch sometime next week. We have a lot to catch up on.", source: "Day 008 교재1", meaning: meanings["catch up on_2"] },
        { week: 2, ko: "이번 주말엔 밀린 잠을 좀 자야겠어. 요즘 계속 너무 피곤해.", en: "I’m going to catch up on sleep this weekend; I’ve been feeling exhausted lately.", source: "Day 008 교재1", meaning: meanings["catch up on_1"] },
        { week: 2, ko: "인스타그램을 하면 최신 패션 트렌드를 파악하는 데 도움이 된다.", en: "Instagram helps me to catch up on the latest fashion trends.", source: "Day 008 교재1", meaning: meanings["catch up on_2"] },
        { week: 2, ko: "읽어야 할 것들이 많이 밀렸어. 만회하는 데만 몇 주는 걸릴 거야.", en: "I am really behind on the reading. It will take me weeks just to catch up.", source: "Day 008 교재1", meaning: meanings["catch up on_1"] },
        { week: 2, ko: "난 그 정도로 힘든 건 없었지. 그냥 밀린 이메일 처리했지.", en: "Nothing as stressful as that; I just caught up on e-mails.", source: "Day 008 교재2", meaning: meanings["catch up on_1"] },
        { week: 2, ko: "음, 토요일에는 그동안 못 쉰 거 몰아서 쉴까 했어.", en: "Well, I was really hoping to catch up on some much-needed rest on Saturday.", source: "Day 008 교재2", meaning: meanings["catch up on_1"] },
        { week: 2, ko: "밀린 강좌 들어야 해. 최근에 좀 해이해졌거든.", en: "I need to catch up on them; I’ve been slacking off lately.", source: "Day 008 교재2", meaning: meanings["catch up on_1"] },
        { week: 2, ko: "첫 월급 받은 것으로 밀린 공과금도 내고 드디어 재정 상태도 다시 안정을 찾아서 기분이 정말 좋았다.", en: "I felt really good catching up on bills with my first paycheck...", source: "Day 008 교재3", meaning: meanings["catch up on_1"] },
        { week: 2, ko: "(할머니 댁에 가서) 할머니 안부는 확인해 본 거야? 고관절 골절이신데 집에 혼자 계시는 게 걱정이 되네.", en: "Have you checked in on Grandma?", source: "Day 009 교재1", meaning: meanings["check on_1"] },
        { week: 2, ko: "식당에서 서빙을 하게 되면 담당 테이블의 손님에게 필요한 것이 있는지 확인해 보아야 한다.", en: "If you are a server at a restaurant, you need to check on diners at your tables to see if they need anything.", source: "Day 009 교재1", meaning: meanings["check on_1"] },
        { week: 2, ko: "오븐에 닭고기 익히고 있는데, (잘 익고 있는지) 확인 좀 해 줄 수 있어?", en: "Could you check on the chicken I’m cooking in the oven?", source: "Day 009 교재1", meaning: meanings["check on_1"] },
        { week: 2, ko: "제 아들이 몸 상태가 안 좋아서 내일 가서 어떤지 확인하고, 식료품이나 약이 필요한지 알아봐야겠습니다.", en: "Since my son has been feeling under the weather, I’m going to check in on him tomorrow...", source: "Day 009 교재1", meaning: meanings["check on_1"] },
        { week: 2, ko: "지난번에 얼굴 본 뒤로 시간이 제법 흘렀네. (자네 사무실에 들러서) 어떻게 지내는지, 그리고 새 직장은 어떤지 한번 볼까 싶어서 연락했어.", en: "It’s been a while since we last spoke, so I thought I’d check in on you and see how you’re doing with the new job.", source: "Day 009 교재1", meaning: meanings["check on_1"] },
        { week: 2, ko: "왜 거기 그냥 서 있는 거예요? 손님에게 음료나 뭐 필요한 게 있는지 어서 테이블을 확인해 보세요.", en: "Why are you just standing there? Go check on your tables and see if they need drinks or anything.", source: "Day 009 교재1", meaning: meanings["check on_1"] },
        { week: 2, ko: "제 상사는 하루에 열두 번씩 (메시지, 통화 등으로) 저를 통제하는 데 이 때문에 미치겠습니다.", en: "My boss checks on me like twelve times a day and it drives me crazy.", source: "Day 009 교재1", meaning: meanings["check on_1"] },
        { week: 2, ko: "며칠 전에 장염 걸렸다고 했잖아요. 그래서 잘 회복하고 있는지 물어보고 싶었어요.", en: "You mentioned that you caught a stomach bug a few days ago, so I wanted to check in and ask how you’re holding up.", source: "Day 009 교재2", meaning: meanings["check on_1"] },
        { week: 2, ko: "챙겨 주셔서 고마워요! 괜찮아요. 아직 침대에 누워 있지만 좋아지고 있어요.", en: "Thanks for checking in on me! I’ve been okay. I’m still stuck in bed but starting to get better.", source: "Day 009 교재2", meaning: meanings["check on_1"] },
        { week: 2, ko: "출발하기 전에 식물들을 확인해야 해. 곧 물을 줘야 할 거야.", en: "I need to check on the plants before we leave. I think they’ll be needing water soon.", source: "Day 009 교재2", meaning: meanings["check on_1"] },
        { week: 2, ko: "303호에 새로 들어온 환자 상태 확인했나요? 차트 보니까 이 환자 주기적인 모니터링이 필요하다고 적혀 있군요.", en: "Have you checked on the new patient in room 303?", source: "Day 009 교재2", meaning: meanings["check on_1"] },
        { week: 2, ko: "동료애를 발휘해서 이들의 안부를 확인하는 것만으로도 큰 변화가 생깁니다.", en: "Giving simple companionship by checking in on them makes a huge difference.", source: "Day 009 교재3", meaning: meanings["check on_1"] },
        { week: 2, ko: "제가 사는 곳의 엘리베이터는 아주 느려요. 제가 15층에 삽니다. 엘리베이터가 내려가는 시간을 이용해서 거울로 외모를 확인하고 보풀이나 고양이 털을 뗍니다.", en: "I use the time coming down to check out my appearance in the mirror and pull off any lint or cat fur.", source: "Day 010 교재1", meaning: meanings["check out_1"] },
        { week: 2, ko: "이 그림 속에 있는 작은 디테일들을 꼼꼼히 봐봐. 이 화가의 독특한 붓 터치가 보일거야.", en: "Check out the tiny details in this painting. You can see the artist’s individual brushstrokes.", source: "Day 010 교재1", meaning: meanings["check out_1"] },
        { week: 2, ko: "〈오징어 게임〉과 〈기생충〉과 같은 한국 미디어에 등장한 유명한 곳을 방문해 보고싶습니다.", en: "We want to check out some of the famous locations from Korean media, like Squid Game and Parasite.", source: "Day 010 교재1", meaning: meanings["check out_1"] },
        { week: 2, ko: "와, 저 여자 옷 좀 봐! 저렇게 입고 이 날씨를 어떻게 견디지?", en: "Wow, check out what she’s wearing!", source: "Day 010 교재1", meaning: meanings["check out_1"] },
        { week: 2, ko: "나랑 손잡고 있으면서 저 여자를 훑어본 거야?", en: "Were you just checking her out while holding my hand?", source: "Day 010 교재1", meaning: meanings["check out_1"] },
        { week: 2, ko: "잠시만. 안에 들어가서 먹을 만한 것 있는지 메뉴 좀 보고 올게.", en: "Hold on. I am gonna go in and check out their menu to see if they have anything good.", source: "Day 010 교재1", meaning: meanings["check out_1"] },
        { week: 2, ko: "용리단길에는 정말 가볼 만한 멋진 곳이 많아.", en: "Yongnidangil has so many cool places to check out.", source: "Day 010 교재1", meaning: meanings["check out_1"] },
        { week: 2, ko: "영업시간과 특별 메뉴와 관련된 최신 정보는 저희 인스타그램을 방문해서 확인해 주시기 바랍니다.", en: "Please check out our Instagram for the latest information on hours and special dishes.", source: "Day 010 교재1", meaning: meanings["check out_1"] },
        { week: 2, ko: "이 시계 자세히 봐봐. 진품이야, 아니면 짝퉁이야?", en: "Check out this watch. Is it genuine or a knock-off?", source: "Day 010 교재1", meaning: meanings["check out_4"] },
        { week: 2, ko: "강연이 너무 지루하고 어려웠다. 불과 몇 분 만에 수업 듣는 학생 대부분이 딴생각을 하기 시작했다.", en: "The lecture was so tedious and complicated. It only took a few minutes before most of the class had mentally checked out.", source: "Day 010 교재1", meaning: meanings["check out_2"] },
        { week: 2, ko: "Samantha는 저랑 헤어지기 한참 전부터 이미 마음이 떠났다고 하더라고요.", en: "Samantha said she had already checked out of the relationship long before she broke up with me.", source: "Day 010 교재1", meaning: meanings["check out_3"] },
        { week: 2, ko: "네, 정말 괜찮은 것 같네요. 그런데 제가 스피커에 대해 아는 게 거의 없어서, 다른 브랜드도 살펴보고 결정해야 할 것 같습니다.", en: "Yeah, that sounds like a real bargain, but I know almost nothing about speakers, so I think I need to check out other brands before I make a decision.", source: "Day 010 교재2", meaning: meanings["check out_1"] },
        { week: 2, ko: "고객들이 저희 매장에서 옷을 입어만 보고는 온라인에서 구매를 하네요.", en: "Customers just use the store to check out clothes before buying them online.", source: "Day 010 교재2", meaning: meanings["check out_1"] },
        { week: 2, ko: "보니까 Jackson이 회의에 집중을 안 하는 듯하네. 이메일 답장한다고 정신이 없는 듯.", en: "It looks like Jackson has checked out of our meeting. Maybe he’s caught up in some e-mails.", source: "Day 010 교재2", meaning: meanings["check out_2"] },
        { week: 2, ko: "어쩌면 내가 이미 아주 오랫동안 감정이 떠난 상태인 걸 수도 있다. 하지만 이런 이야기를 어떻게 꺼내야 할지 모르겠다.", en: "Maybe I’ve been emotionally checked out for too long, but I don’t know how to bring this up with him.", source: "Day 010 교재3", meaning: meanings["check out_3"] }
    ];

    // 3. 영어회화 데이터 (Week 1 + 2)
    const rawConvData = [
        // Week 1
        { week: 1, source: "Day001 교재1", ko: "저는 재택근무 체질이 아니에요. 늘 딴짓하게 되거든요", en: "Working from home isn’t for me. I always get distracted." },
        { week: 1, source: "Day001 교재1", ko: "소개팅은 저랑 안 맞아요.", en: "Going on blind dates isn’t for me." },
        { week: 1, source: "Day001 교재1", ko: "노트북은 저랑 좀 안 맞아요. 키보드가 뭔가 엄청 불편하거든요.", en: "Laptops aren’t really for me. Something about the keyboards is super uncomfortable." },
        { week: 1, source: "Day001 교재1", ko: "전기차는 좀 별로예요. 충전소는 요즘 늘었지만, 여전히 엄청 귀찮게 느껴져요.", en: "Electric cars aren’t for me. We have more charging stations around now, but it still feels like too much of a hassle." },
        { week: 1, source: "Day001 교재1", ko: "그 사람 직업이 좋은 건 아는데, 그런 남자는 나는 별로야.", en: "I know he has a decent job, but guys like him aren’t really for me." },
        { week: 1, source: "Day001 교재2", ko: "우리 나가서 맛난 회 먹을까? 내가 살 게.", en: "Why don’t we go out and get some nice sashimi? My treat!" },
        { week: 1, source: "Day001 교재2", ko: "너무 고맙긴 한데. 난 회를 별로 안 좋아해. 식감이 적응이 안 돼.", en: "It’s kind of you to offer, but raw fish just isn’t for me. I can’t get used to the texture." },
        { week: 1, source: "Day001 교재2", ko: "청취 연습을 위해 <기묘한 이야기>를 시청할 것을 추천합니다.", en: "I recommend watching Stranger Things to practice listening." },
        { week: 1, source: "Day001 교재2", ko: "좋은 생각이긴 한데, 저는 미국 프로그램이 체질에 안 맞아요. 스토리에 재미가 안 붙어요.", en: "It’s a good idea, but American shows aren’t for me. I can’t really get into the stories." },
        { week: 1, source: "Day001 교재2", ko: "애들하고 정말 잘 노는군요. 선생님 할 생각은 해 보셨나요?", en: "You’re really great around kids. Have you ever thought of being a teacher?" },
        { week: 1, source: "Day001 교재2", ko: "아니요. 저는 가르치는 거랑 잘 안 맞아요. 애들이랑 노는 건 좋은데, 공부시키는 게 너무 힘들 듯해요", en: "No, no. Teaching isn’t really for me. I like to play with them but trying to make them study seems like hard work." },
        { week: 1, source: "Day001 교재3", ko: "안녕, Greg. 내가 생일 선물로 받은 로잉 머신 기억하지? 혹시 관심 있어? 나랑은 별로 안 맞더라고", en: "Hey, Greg. Do you remember that rowing machine I got for my birthday? Are you interested in it? Turns out it’s not really for me." },
        { week: 1, source: "Day001 교재4", ko: "좋아 보인다. 편안해 보이고. 학과장을 안 하는 게 너랑 맞는 거야", en: "You look good. Relaxed. Not being chair suits you." },
        { week: 1, source: "Day001 교재4", ko: "혼자 일하는 건 나랑 안 맞는다는 걸 느꼈어. 사람들이랑 같이 있어야 진짜로 능률이 오르거든.", en: "I’ve found that working on my own doesn’t really suit me. I need to be around other people to really be productive." },
        { week: 1, source: "Day001 교재4", ko: "있잖아. 재택근무는 내 체질이 아니야.", en: "You know what? Working from home doesn’t really work for me." },
        { week: 1, source: "Day001 대표", ko: "재택근무는 저랑 안 맞아요.", en: "Working from home isn’t for me." },
        { week: 1, source: "Day002 교재1", ko: "다음 에피소드는 어떤 내용일지 궁금해 미치겠어.", en: "I can’t wait to see what the next episode will bring." },
        { week: 1, source: "Day002 교재1", ko: "아내가 제 선물을 개봉할 때 어떤 표정일지 궁금해 죽겠습니다.", en: "I can’t wait to see the look on my wife’s face when she opens my gift." },
        { week: 1, source: "Day002 교재1", ko: "이 프로젝트가 빨리 끝났으면 좋겠어요. 너무 오래 걸립니다.", en: "I can’t wait to be done with this project. It’s taking forever." },
        { week: 1, source: "Day002 교재1", ko: "여보, 저녁 식사가 너무 맛있는 냄새가 나네. 어서 먹고 싶어.", en: "That dinner smells delicious, honey. I can’t wait." },
        { week: 1, source: "Day002 교재1", ko: "<베이비 드라이버>가 미국에서는 몇 달 전에 개봉했어. 이곳에서도 어서 개봉했으면 좋겠다.", en: "Baby Driver was released months ago in the United States. I can’t wait for it to come out here." },
        { week: 1, source: "Day002 교재2", ko: "그 책 드디어 영화로 만들었다며?", en: "Did you hear they finally made that book into a movie?" },
        { week: 1, source: "Day002 교재2", ko: "응! 어서 보고 싶어. 내가 제일 좋아하는 장면들이 다 포함되어 있기를.", en: "Yes! I can’t wait to see it. I hope they included all my favorite scenes." },
        { week: 1, source: "Day002 교재2", ko: "프로젝트는 잘 되어 가나요? 한동안 매달려 있으신 것 같은데.", en: "How’s that project going? It seems like you’ve been working on it for a while." },
        { week: 1, source: "Day002 교재2", ko: "네, 일주일 내내 이것을 하고 있습니다. 어서 끝내고 뭔가 다른 걸로 넘어가고 싶어요.", en: "Yeah, I’ve been working on it all week. I can’t wait to finish it and finally move on to something else." },
        { week: 1, source: "Day002 교재2", ko: "네가 뜨개질할 수 있는 걸 몰랐네. 뭐 만들고 있니?", en: "I didn’t know you could knit. What are you making?" },
        { week: 1, source: "Day002 교재2", ko: "여동생에게 줄 스카프를 만들고 있어. 내가 자기 주려고 이걸 만든 걸 알면 어떤 표정일까 궁금해 죽겠어.", en: "I’m making a scarf for my little sister. I can’t wait to see the look on her face when she realizes I made this for her." },
        { week: 1, source: "Day002 교재3", ko: "신형 그랜저를 어서 보고 싶네요. 위장막 때문에 스파이 샷이 의미가 없어서 너무 속상하네요.", en: "I can’t wait to get a glimpse of the new Grandeur. Hyundai uses these car camouflage wraps. They make spy shots useless and it’s really frustrating." },
        { week: 1, source: "Day002 교재4", ko: "(소개팅 상황에서) 그 여성분 어서 만나 보고 싶어.", en: "I’m really anxious to meet her." },
        { week: 1, source: "Day002 교재4", ko: "어서 집에 가서 선물을 개봉해 보고 싶다.", en: "I’m anxious to get home to open my presents." },
        { week: 1, source: "Day002 교재4", ko: "(이메일에서) 말씀 많이 들었습니다. 하루빨리 함께 일하고 싶습니다.", en: "I’ve heard a great deal about you. I look forward to working with you." },
        { week: 1, source: "Day002 교재4", ko: "안 그래도 그 콘서트 너무 기대됐는데, 아이유도 나온다고 하니까 더 기대된다.", en: "I was looking forward to the concert, but now even more so, since I heard IU will be there." },
        { week: 1, source: "Day002 대표", ko: "하루빨리 새 집으로 이사 가고 싶어요.", en: "I can’t wait to move into the new house." },
        { week: 1, source: "Day003 교재1", ko: "제가 마지막 남은 피자 한 조각 먹어도 될까요?", en: "Do you mind if I finish off the last piece of pizza?" },
        { week: 1, source: "Day003 교재1", ko: "미안한데, 오는 길에 커피 좀 사다 줄 수 있나요?", en: "Do you mind grabbing me some coffee on your way?" },
        { week: 1, source: "Day003 교재1", ko: "제가 여유 시간이 겨우 5분 있어요. 짧게 해 주실 수 있을까요?", en: "I’ve got only five minutes to spare. Do you mind keeping it short?" },
        { week: 1, source: "Day003 교재1", ko: "에어컨 좀 약하게 하면 안 될까요? 좀 추워서요.", en: "Do you mind turning down the air-conditioning? I feel a bit cold." },
        { week: 1, source: "Day003 교재1", ko: "개인적인 질문 하나 해도 될까요?", en: "Do you mind if I ask you a personal question?" },
        { week: 1, source: "Day003 교재2", ko: "죄송한데, 회의를 금요일로 옮겨도 될까요?", en: "Do you mind if we move the meeting to Friday?" },
        { week: 1, source: "Day003 교재2", ko: "네, 괜찮습니다. 사실 저희에겐 금요일이 더 좋아요.", en: "Sure. Friday works better for us, actually." },
        { week: 1, source: "Day003 교재2", ko: "죄송한데, 꼭대기 선반에 있는 저 시리얼 상자들 중 하나를 내려 줄 수 있을까요?", en: "Excuse me, do you mind grabbing me one of those cereal boxes on the top shelf?" },
        { week: 1, source: "Day003 교재2", ko: "당연하죠. 얼마든지요", en: "Sure. Always happy to help!" },
        { week: 1, source: "Day003 교재2", ko: "어디서 만나면 될까요? (비즈니스적)", en: "Where would you like to meet?" },
        { week: 1, source: "Day003 교재2", ko: "제가 그쪽 사무실로 가도 상관없습니다.", en: "I don’t mind coming over to your office." },
        { week: 1, source: "Day003 교재3", ko: "괜찮으시면 혹시 모르니까 이번에는 2시 30분에 시작해도 될는지요?", en: "If you don’t mind, could we start at 2:30 this time, just to be safe?" },
        { week: 1, source: "Day003 교재4", ko: "제가 이번 주 금요일에 이사 나가요. 미안하지만 좀 도와주실 수 있을까요?", en: "I’m moving out this Friday. Would you mind giving me a hand?" },
        { week: 1, source: "Day003 교재4", ko: "슬슬 배가 고프네요. 잠깐 나가서 간단히 뭐 좀 먹고 와도 될까요?", en: "I’m starting to get a bit hungry. Would you mind if I stepped out for a moment and grabbed a bite to eat?" },
        { week: 1, source: "Day003 대표", ko: "죄송한데 조금 짧게 해 주시겠어요?", en: "Do you mind keeping it a bit short?" },
        { week: 1, source: "Day004 교재1", ko: "그 여자분 키 엄청 커요.", en: "She is super tall." },
        { week: 1, source: "Day004 교재1", ko: "그 사람이 무지 바쁘거나, 아니면 저에 대한 관심이 식고 있는 거겠죠.", en: "Either he has been super busy, or he is losing interest in me." },
        { week: 1, source: "Day004 교재1", ko: "제가 요즘 이사 준비 때문에 엄청 바빴어요.", en: "I’ve been super busy with my upcoming move." },
        { week: 1, source: "Day004 교재1", ko: "우와. 연세 있으신 분치고는 몸매가 너무 좋으시네요.", en: "Wow. You’re in super good shape for an old guy." },
        { week: 1, source: "Day004 교재1", ko: "서울은 어디라도 다 너무 비싸. 근데 후암동은 상대적으로 저렴한 편이지", en: "All the neighborhoods in Seoul are super expensive, but Huam-dong is relatively cheap" },
        { week: 1, source: "Day004 교재2", ko: "무슨 점심값이 만 원이 넘는 거야.", en: "I never thought I’d have to pay over 10,000 won for lunch." },
        { week: 1, source: "Day004 교재2", ko: "그러게. 요새 물가가 너무너무 비싸.", en: "Yeah. Everything is getting super expensive." },
        { week: 1, source: "Day004 교재2", ko: "오! 그럼 귤 한 박스 사다 줄래? 지금 제철이니 엄청 쌀 거야.", en: "Oh! How about a box of tangerines? They should be super cheap since they’re in-season." },
        { week: 1, source: "Day004 교재2", ko: "11월 말치고는 너무 따뜻하다. 지금쯤이면 보통은 훨씬 더 추운데.", en: "It’s unusually warm for late November. It’s usually much colder by now." },
        { week: 1, source: "Day004 교재2", ko: "맞아. 가을이 점점 짧아지고는 있는데 올해는 엄청 길다.", en: "Right. Autumn has been getting shorter, but this year, it’s been super long." },
        { week: 1, source: "Day004 교재3", ko: "이번 침대 프레임 조립은 정말 쉽더군요. 조립하는 데 한 시간도 안 걸렸습니다.", en: "this bed frame was super easy to put together. It took me less than an hour." },
        { week: 1, source: "Day004 교재4", ko: "그 삼겹살집은 맛은 괜찮은 편인데 가격이 상당히 비싸다.", en: "The pork belly place is pretty good, but it’s quite expensive." },
        { week: 1, source: "Day004 교재4", ko: "당신은 영어를 상당히 잘하는군요.", en: "Your English is quite good." },
        { week: 1, source: "Day004 대표", ko: "물가가 올라도 너무 올라요.", en: "Everything is getting super expensive." },
        { week: 1, source: "Day005 교재1", ko: "중매업체에 등록해 보는 게 어때요?", en: "How do you feel about signing up for a matchmaking service?" },
        { week: 1, source: "Day005 교재1", ko: "교회에 가 보는 게 어때요?", en: "How do you feel about going to church?" },
        { week: 1, source: "Day005 교재1", ko: "등산 모임에 가입해 보는 게 어떨까요?", en: "How do you feel about joining a hiking club?" },
        { week: 1, source: "Day005 교재1", ko: "성형 수술 하는 거 어떻게 생각하세요?", en: "How do you feel about plastic surgery?" },
        { week: 1, source: "Day005 교재2", ko: "코치가 전 동료였는데, 그런 팀에 합류하는 기분이 어떠신가요?", en: "How do you feel about joining a team when the coach is your ex-teammate?" },
        { week: 1, source: "Day005 교재2", ko: "저녁 먹고 우리 집에 가서 <컨저링> 볼까 하는데. 공포 영화 어때?", en: "After dinner, I was thinking we could go to my place and watch The Conjuring. How do you feel about horror movies?" },
        { week: 1, source: "Day005 교재2", ko: "싫어, 공포 영화는 못 보겠어. 무서운 거 보는 게 뭐가 재미있다고.", en: "No, I can’t stand horror movies! Watching something scary isn’t my idea of fun." },
        { week: 1, source: "Day005 교재3", ko: "Frank 팀장님 밑에서 일하니까 어떤가요? 이동 제안을 진지하게 고민해보기 전에 우선 당신의 경험을 듣고 싶습니다.", en: "How do you feel about working under Frank? I want to hear about your experience before I really consider his transfer offer." },
        { week: 1, source: "Day005 교재4", ko: "모든 게 완전 비싸네.", en: "Everything is totally overpriced." },
        { week: 1, source: "Day005 교재4", ko: "완전 깜박했어.", en: "It totally slipped my mind." },
        { week: 1, source: "Day005 교재4", ko: "양복 입으니까 완전 딴 사람 같네.", en: "You look totally different in a suit." },
        { week: 1, source: "Day005 교재4", ko: "완전 괜찮아.", en: "That’s totally fine." },
        { week: 1, source: "Day005 교재4", ko: "괜히 고치느라 애쓰지 마. 완전히 고장이 났으니까.", en: "Don’t bother trying to fix it. It’s totally broken." },
        { week: 1, source: "Day005 교재4", ko: "그 영화는 무조건 아이맥스로 봐야 해.", en: "You should totally go see the movie in IMAX." },
        { week: 1, source: "Day005 교재4", ko: "난 초밥이 너무 땡겨.", en: "I could totally go for some sushi." },
        
        // Week 2
        { week: 2, source: "Day006 교재1", ko: "안 좋았던 한 주를 날려 버리려면 친구들과 맛있는 식사를 하는 게 최고지.", en: "There’s nothing like a nice meal with friends to turn a bad week around." },
        { week: 2, source: "Day006 교재1", ko: "주말 내내 넷플릭스 드라마 보는 게 최고야.", en: "There’s nothing like binging a show on Netflix all weekend." },
        { week: 2, source: "Day006 교재1", ko: "크리스마스에는 칠면조 저녁 식사와 풍미 좋은 와인만 한 게 없지.", en: "There is nothing like a turkey dinner and spiced wine for Christmas." },
        { week: 2, source: "Day006 교재1", ko: "다시 콘서트에 갈 수 있어서 너무 좋아. 가장 좋아하는 밴드의 라이브 공연을 보는 것만큼 좋은 것은 없지.", en: "I’m so glad we can go to concerts again. There’s nothing like seeing your favorite band live." },
        { week: 2, source: "Day006 교재1", ko: "팬케이크에는 진짜 메이플 시럽을 얹어 먹어야 제맛이야.", en: "There’s nothing like real maple syrup on pancakes." },
        { week: 2, source: "Day006 교재2", ko: "회사에서 힘든 하루를 보내고 나면 시원한 맥주가 최고지. 퇴근하고 한잔하러 갈까?", en: "There’s nothing like a cold beer after a long day of work. How about we go grab one when we get off?" },
        { week: 2, source: "Day006 교재2", ko: "좋지! 네가 산다면.", en: "Sure! As long as you’re buying." },
        { week: 2, source: "Day006 교재2", ko: "내가 좀 쳐져 보인다면 미안. 남자 친구랑 잠시 안 보기로 했거든. 근데 정말 보고 싶어.", en: "Sorry if I seem a little depressed. My boyfriend and I decided to take a little break. I really miss him." },
        { week: 2, source: "Day006 교재2", ko: "아, 그랬구나. 그럼 쇼핑하러 가자. 내가 널 알잖아. 기분 전환에는 옷 사는 게 최고야.", en: "Aww, I’m sorry. Come on, let’s go shopping. I know you. There’s nothing like buying clothes to cheer you up." },
        { week: 2, source: "Day006 교재2", ko: "어서 집에 가고 싶어. 남편이 특별한 음식을 해 준다고 했거든", en: "I can’t wait to get home. My husband said he would cook me something special." },
        { week: 2, source: "Day006 교재2", ko: "오, 멋지다! 힘든 하루를 보낸 후에는 기운을 차리는 데 집밥만 한 게 없지.", en: "Oh, that’s perfect then! There’s nothing like a home-cooked meal to lift your spirits after a long day." },
        { week: 2, source: "Day006 교재3", ko: "바디프렌드 매장에 오셔서 새로 출시된 프로 마사지 X7 의자를 체험해 보세요. 일주일에 세 번 전문 마사지사를 집에 부른다고 해도 저희 의자로 받는 안마가 최고라는 점을 인정할 수밖에 없을 겁니다.", en: "Come to a Bodyfriend store and try out our new line of Pro Massage X7 chairs. Even if you have a professional masseur come to your house three times a week, you’ll have to admit there’s nothing like a massage from one of our chairs." },
        { week: 2, source: "Day006 교재4", ko: "서울에서는 이 집 빵이 최고야.", en: "You can’t find any better bread in Seoul." },
        { week: 2, source: "Day006 교재4", ko: "서핑하기에는 오늘이 딱이다.", en: "I can’t imagine a better day for surfing." },
        { week: 2, source: "Day006 교재4", ko: "요즘같이 장사가 안 된 적도 없어요.", en: "Business has never been any worse." },
        { week: 2, source: "Day006 교재4", ko: "이건 Sarah가 가장 잘 알지.", en: "No one knows this better than Sarah." },
        { week: 2, source: "Day006 교재4", ko: "지난 분기에 가장 많은 차를 판매했다.", en: "They sold more vehicles last quarter than they ever have." },
        { week: 2, source: "Day006 교재4", ko: "카카오톡이 한국에서 가장 성공한 플랫폼인 것 같습니다.", en: "No other platform in Korea seems more successful than KakaoTalk." },
        { week: 2, source: "Day006 대표", ko: "재충전에는 캠핑만 한 게 없죠.", en: "There is nothing like camping to recharge your batteries." },
        { week: 2, source: "Day007 교재1", ko: "미슐랭 스타를 받은 음식이라면 뭐든 좋아.", en: "I’m up for anything with a Michelin star." },
        { week: 2, source: "Day007 교재1", ko: "뭐 하고 싶어? 난 뭐든 다 좋아.", en: "What do you feel like doing? I’d be up for just about anything." },
        { week: 2, source: "Day007 교재1", ko: "나 비어 퐁 파트너 찾고 있는데. 관심 있어?", en: "I’m looking for a beer pong partner. Are you down?" },
        { week: 2, source: "Day007 교재1", ko: "나 프라이드치킨이 무지 먹고 싶어. 오늘 밤에 같이 먹을 사람?", en: "I’ve been craving fried chicken. Is anyone down for some tonight?" },
        { week: 2, source: "Day007 교재1", ko: "토요일 아침에 북한산 등산 갈까 하는데 같이 갈 사람이 필요해. 관심 있을까?", en: "I was thinking of hiking Bukhan Mountain on Saturday morning, and I need a buddy. Do you think you’d be down?" },
        { week: 2, source: "Day007 교재2", ko: "오늘 저녁에 뭐 먹고 싶어?", en: "What do you want to have tonight?" },
        { week: 2, source: "Day007 교재2", ko: "뭐라도 좋아. 너무 매운 것만 아니면.", en: "I am up for anything, as long as it’s not too spicy." },
        { week: 2, source: "Day007 교재2", ko: "안녕 얘들아, 나랑 고든 램지 버거 먹으러 갈 사람 있을까?", en: "Hey guys‒anyone want to go with me to try Gordon Ramsey’s burger place?" },
        { week: 2, source: "Day007 교재2", ko: "나 갈게! 네가 산다면 말이야.", en: "I’m down, as long as you’re paying." },
        { week: 2, source: "Day007 교재2", ko: "얘들아, 오늘 밤에 애들 집에 불러서 게임할까 하는데. 같이 할 사람?", en: "Guys, I was thinking about having people over for a game night. Who’s in?" },
        { week: 2, source: "Day007 교재2", ko: "음, 이따가 약속이 있긴 한데, 얼굴 정도는 비출 수 있어.", en: "Well, I have plans later, but I am down for saying hello, at least." },
        { week: 2, source: "Day007 교재3", ko: "일생일대의 모험을 즐기고 싶으신가요? 에베레스트 등반을 원하신다면 네팔 어드벤처를 선택해 주세요. 이번 한 달 동안만 신규 고객을 위한 특별 행사가 준비되어 있습니다. 선착순 50인은 무료로 여행 보험에 가입할 수 있습니다. 다른 고객이 채 가기 전에 지금 예약하세요.", en: "Are you down for the adventure of a lifetime? Choose Nepal Adventures for your Everest climb. We have a special offer for new customers available only this month. The first fifty hikers who sign up will receive free travel insurance. Act now, before someone else takes your spot" },
        { week: 2, source: "Day007 교재4", ko: "아무거나 좋아요.", en: "Anything sounds great." },
        { week: 2, source: "Day007 교재4", ko: "아무거나 괜찮아요.", en: "Anything would be fine." },
        { week: 2, source: "Day007 교재4", ko: "(너무) 좋습니다.", en: "That would be (so) nice." },
        { week: 2, source: "Day007 교재4", ko: "나랑 같이 가는 거 어때?", en: "Are you interested in coming with me?" },
        { week: 2, source: "Day007 교재4", ko: "너무 좋지.", en: "That would be nice." },
        { week: 2, source: "Day007 교재4", ko: "좋은 생각이야.", en: "That sounds like a good idea." },
        { week: 2, source: "Day007 대표", ko: "너무 매운 것만 아니면 뭐든 다 좋아요", en: "I am up for anything, as long as it’s not too spicy" },
        { week: 2, source: "Day008 교재1", ko: "나도 가고는 싶은데, 오늘 몸이 좀 안 좋아.", en: "I wish I could come, but I don’t feel quite right today." },
        { week: 2, source: "Day008 교재1", ko: "저녁을 같이 못 할 것 같습니다. 오늘 몸이 좀 안 좋네요.", en: "I’m afraid I can’t join you for dinner. I don’t feel quite right today." },
        { week: 2, source: "Day008 교재1", ko: "여보, 나 오늘 몸이 좀 안 좋아. 수진이 학교에서 좀 데려와 줄래?", en: "Honey, I don’t feel quite right today. Can you pick up Sujin from school?" },
        { week: 2, source: "Day008 교재1", ko: "오늘은 저녁 안 먹을래. 오늘 속이 좀 안 좋아서.", en: "I think I’ll skip dinner. My stomach doesn’t feel quite right today." },
        { week: 2, source: "Day008 교재1", ko: "너희 딸 파티에 나는 안 가는 게 좋을 것 같아. 오늘 몸이 좀 안 좋네.", en: "I don’t think it’s a good idea for me to come to your daughter’s party. I don’t feel quite right today" },
        { week: 2, source: "Day008 교재2", ko: "뭔지는 모르겠는데, 오늘 몸이 좀 안 좋아.", en: "I’m not sure what it is, but I don’t feel quite right today." },
        { week: 2, source: "Day008 교재2", ko: "아침에 먹은 국에 문제가 있었을지도. 좀 상한 냄새가 났거든.", en: "Maybe there was something wrong with that soup you had for breakfast. It smelled a little funny." },
        { week: 2, source: "Day008 교재2", ko: "Sam, 우리 점심 약속 유효한 거지?", en: "Sam, are you still able to meet for lunch?" },
        { week: 2, source: "Day008 교재2", ko: "미안해, 사실 오늘 몸이 좀 안 좋아. 집에 있어야 할 것 같아. 다음에 먹어도 될까?", en: "Sorry, actually I don’t feel quite right today. I think I need to stay home. Can we take a rain check?" },
        { week: 2, source: "Day008 교재2", ko: "오늘 밤에 우리 밖에 나가 놀기로 한 건 아는데, 오늘 뭔가 몸이 좀 이상해.", en: "I know we’re supposed to go out tonight, but I don’t feel quite right today." },
        { week: 2, source: "Day008 교재2", ko: "이런. 괜찮은 거야? 그냥 음식 포장해 와서 집에서 영화 보면서 쉬는 건 어때?", en: "Oh, no. Are you okay? How about we get takeout and rest at home with a movie instead?" },
        { week: 2, source: "Day008 교재3", ko: "안녕하세요, Brian. 샌프란시스코 사무실은 요즘 어때요? 지난주에 코로나 걸렸다고 들었는데, 안부 확인차 연락드렸어요. 사실 저도 어제 몸이 안 좋아서 검사를 했는데 다행히 음성으로 나왔습니다", en: "Hi, Brian. How are things going over there in the San Francisco office? I heard that you caught COVID last week, and so I wanted to check in and ask how you’re doing. Actually, I wasn’t feeling quite right myself yesterday. I got tested and thankfully, it turned out negative." },
        { week: 2, source: "Day008 교재4", ko: "몸이 조금 아프네.", en: "I feel a little bit sick." },
        { week: 2, source: "Day008 교재4", ko: "몸이 너무 안 좋아.", en: "I am really not feeling well." },
        { week: 2, source: "Day008 교재4", ko: "몸이 좀 안 좋아.", en: "I am not really feeling well." },
        { week: 2, source: "Day008 교재4", ko: "다행히 많이 나아졌어.", en: "I am feeling much better thankfully." },
        { week: 2, source: "Day008 교재4", ko: "계속 기운이 좀 없네.", en: "I have been feeling low." },
        { week: 2, source: "Day008 교재4", ko: "감기 기운이 있다.", en: "I feel like I’m coming down with a cold." },
        { week: 2, source: "Day008 교재4", ko: "감기인지 뭔지는 모르지만 뭔가 몸이 안 좋다.", en: "I feel like I’m coming down with something." },
        { week: 2, source: "Day008 대표", ko: "오늘 몸이 좀 안 좋아요.", en: "I don’t feel quite right today." },
        { week: 2, source: "Day009 교재1", ko: "제가 첫 번째 문장을 읽을까요?", en: "Would you like me to read the first sentence?" },
        { week: 2, source: "Day009 교재1", ko: "제가 일어난 김에 물 좀 가져다드릴까요?", en: "Would you like me to get you some water while I’m up?" },
        { week: 2, source: "Day009 교재1", ko: "수정본 보내드릴까요?", en: "Would you like me to send you the revised version?" },
        { week: 2, source: "Day009 교재1", ko: "내가 따라가 줄까?", en: "Do you want me to come along with you?" },
        { week: 2, source: "Day009 교재1", ko: "내가 너희 둘 자리 마련해 줄까?", en: "Do you want me to set you two up on a date?" },
        { week: 2, source: "Day009 교재2", ko: "지금 역에서 걸어가고 있는데요. 커피 사다 드릴까요?", en: "I’m walking from the station. Would you like me to pick up any coffee?" },
        { week: 2, source: "Day009 교재2", ko: "좋습니다. 고마워요.", en: "That would be great. Thanks." },
        { week: 2, source: "Day009 교재2", ko: "아직 다 하지는 못했는데, 지금까지 작업한 거 보내 드릴까요?", en: "I’m not done yet, but would you like me to send you what I have so far?" },
        { week: 2, source: "Day009 교재2", ko: "고마워요. 그러면 언제까지 완성할 수 있을 것 같나요?", en: "Thanks. So, when do you think you will be able to complete the materials?" },
        { week: 2, source: "Day009 교재2", ko: "방금 인천에 도착했어요! 그나저나 면세점에 들를까 하는데요, 아빠랑 엄마 뭐 사다 드릴까요?", en: "I just landed at Incheon! By the way, I think I’ll go by a duty-free shop. Do you want me to get you or Mom anything?" },
        { week: 2, source: "Day009 교재2", ko: "말만이라도 고맙다, 내 딸.", en: "Thanks for offering, Sweetie." },
        { week: 2, source: "Day009 교재3", ko: "안녕하세요. 다름이 아니라 저희 수요일에 회의하는 거 맞는지 확인차 연락드립니다. 그리고 용산 본사에 회의실 잡을까요? (잠시 후) 근데 보니까 본사 회의실은 예약이 꽉 찼네요. 장소를 변경하든지, 날짜를 바꾸든지 해야 할 것 같습니다.", en: "I just wanted to make sure the meeting is still on for Wed. And would you like me to arrange a room at the headquarters in Yongsan? (pause) I just found out that headquarters is all booked up. We’ll have to change something, either the location or date." },
        { week: 2, source: "Day009 교재4", ko: "공항에 마중 나와 줘.", en: "I want you to pick me up at the airport." },
        { week: 2, source: "Day009 교재4", ko: "냄새 심해지기 전에 쓰레기 밖에 좀 내다버려 줘.", en: "I want you to take out the trash before it gets too smelly." },
        { week: 2, source: "Day009 교재4", ko: "Jeff와 회의 잡아 주십시오.", en: "I’d like you to set up a meeting with Jeff." },
        { week: 2, source: "Day009 교재4", ko: "우리 같이 회의록 살펴볼까요?", en: "I’d like us to go over the minutes." },
        { week: 2, source: "Day009 교재4", ko: "이런 가족 식사 자리를 좀 더 자주 가졌으면 해요.", en: "I’d like us to have more family dinners together like this." },
        { week: 2, source: "Day009 대표", ko: "저 지금 스타벅스인데 커피 사다 드릴까요?", en: "Would you like me to grab you some coffee while I’m at Starbucks?" },
        { week: 2, source: "Day010 교재1", ko: "주연 배우로 생각하고 있는 분 있으신가요?", en: "Do you have any actor in mind for the lead role?" },
        { week: 2, source: "Day010 교재1", ko: "괜찮은 소고깃집 생각해 둔 데 있어?", en: "Do you have any good beef place in mind?" },
        { week: 2, source: "Day010 교재1", ko: "딱히 염두에 둔 차는 없습니다. 상태만 좋으면 뭐라도 사겠습니다.", en: "I don’t really have any car in mind. I will go with pretty much anything as long as it’s in good shape." },
        { week: 2, source: "Day010 교재1", ko: "특별히 염두에 둔 건 없습니다.", en: "I have nothing particular in mind." },
        { week: 2, source: "Day010 교재1", ko: "다른 안이 없으시면 제가 제안을 하고 싶습니다.", en: "I’d like to make a suggestion, unless you have something in mind." },
        { week: 2, source: "Day010 교재2", ko: "가격대는 어느 정도 생각하세요?", en: "What price range do you have in mind?" },
        { week: 2, source: "Day010 교재2", ko: "십만 원 미만이면 다 괜찮아요.", en: "Anything under 100,000 won would be fine." },
        { week: 2, source: "Day010 교재2", ko: "포르쉐를 받으려면 2년을 기다리셔야 합니다. 그것도 보증금으로 오백만 원을 걸 때 이야기고요.", en: "You will have to wait two years to get a Porsche. And that’s only if you put down five million won as a deposit." },
        { week: 2, source: "Day010 교재2", ko: "그건 문제가 되지 않아요. 전 포르쉐 외에는 살 생각이 없거든요.", en: "It really doesn’t matter. A Porsche is the only car I have in mind." },
        { week: 2, source: "Day010 교재2", ko: "올해 여름휴가는 어디로 가고 싶어?", en: "Where do you want to go for our summer vacation this year?" },
        { week: 2, source: "Day010 교재2", ko: "몇 군데 생각하고 있는 곳이 있는데, 장시간 비행기 타도 괜찮아?", en: "I have a few places in mind. Are you okay taking a long flight?" },
        { week: 2, source: "Day010 교재3", ko: "워크숍 장소로 다음 두 곳을 생각하고 있습니다. 하지만 더 나은 곳이 있으면 알려 주십시오. 꼭 고려해 보겠습니다. 그나저나 조 인사팀장이 기조연설을 못 하게 되었다면서요. 팀 내에서 대신할 분 누구 염두에 두고 계신가요?", en: "We have the following two places in mind as possible sites for the workshop. However, if you have any suggestions for places that would be more suitable, please let us know. We’ll definitely take them into consideration. By the way, I heard that your HR manager, Ms. Cho, is no longer available to deliver the keynote speech. Does your team have anyone in mind to replace her?" },
        { week: 2, source: "Day010 교재4", ko: "연필 놓고 앞쪽으로 시험지를 제출하세요.", en: "Put your pencils down and hand your papers to the front." },
        { week: 2, source: "Day010 교재4", ko: "보증금으로 최소 50%는 걸어 두셔야 합니다.", en: "You have to put down at least 50%." },
        { week: 2, source: "Day010 교재4", ko: "내가 보기엔 네 이력서에 쓰기에 알콘에서의 인턴 경력이 너무 짧은 것 같아.", en: "I think your internship experience at Alcon is too short to put down on your résumé." },
        { week: 2, source: "Day010 교재4", ko: "건강보험 칸은 그냥 비워 두셨군요. 건강보험 미가입자인가요?", en: "You didn’t put down anything in the health insurance section. Does that mean you’re uninsured?" },
        { week: 2, source: "Day010 대표", ko: "가격대는 어느 정도 생각하세요?", en: "What price range do you have in mind?" }
    ];

    // 영어회화 난이도 분리
    const convEasy = rawConvData.filter(item => item.source.includes('대표') || item.source.includes('교재1'));
    const convHard = rawConvData.filter(item => !(item.source.includes('대표') || item.source.includes('교재1')));

    // 퀴즈 진행 상태 변수
    let currentQuestions = [];
    let currentIndex = 0;
    let isPhrasalMode = false;
    let starredQuestions = []; 

    function shuffleArray(array) {
        let shuffled = [...array];
        for (let i = shuffled.length - 1; i > 0; i--) {
            const j = Math.floor(Math.random() * (i + 1));
            [shuffled[i], shuffled[j]] = [shuffled[j], shuffled[i]];
        }
        return shuffled;
    }

    // 선택된 주차에 맞게 필터링
    function filterByWeek(dataArray) {
        if (currentWeekFilter === 'all') {
            return dataArray;
        }
        return dataArray.filter(item => item.week === currentWeekFilter);
    }

    function startQuiz(mode) {
        let sourceArray = [];
        let titleMode = "";

        if (mode === 'phrasal-easy') { 
            sourceArray = phrasalEasy; isPhrasalMode = true; 
            titleMode = "🟢 구동사 순한맛 퀴즈"; 
        }
        else if (mode === 'phrasal-hard') { 
            sourceArray = phrasalHard; isPhrasalMode = true; 
            titleMode = "🔴 구동사 매운맛 퀴즈"; 
        }
        else if (mode === 'conv-easy') { 
            sourceArray = convEasy; isPhrasalMode = false; 
            titleMode = "💬 영어회화 순한맛 퀴즈"; 
        }
        else if (mode === 'conv-hard') { 
            sourceArray = convHard; isPhrasalMode = false; 
            titleMode = "🔥 영어회화 매운맛 퀴즈"; 
        }

        const filteredData = filterByWeek(sourceArray);

        if (filteredData.length === 0) {
            alert(`선택하신 주차(${currentWeekFilter === 'all' ? '누적' : currentWeekFilter + '주 차'})에 해당하는 데이터가 아직 없습니다.`);
            return;
        }

        document.getElementById('mode-selection').classList.add('hidden');
        document.getElementById('quiz-area').classList.remove('hidden');
        
        let weekText = currentWeekFilter === 'all' ? " (1~2주차 누적)" : ` (Week ${currentWeekFilter})`;
        document.getElementById('main-title').innerText = titleMode + weekText; 
        
        starredQuestions = []; 
        // 7문제만 추출 (데이터가 7개 미만이면 전체 추출)
        const qCount = Math.min(filteredData.length, 7);
        currentQuestions = shuffleArray(filteredData).slice(0, qCount);
        currentIndex = 0;
        
        loadQuestion();
    }

    function loadQuestion() {
        document.getElementById('answer-section').classList.add('hidden');
        document.getElementById('btn-next').classList.add('hidden');
        document.getElementById('btn-finish').classList.add('hidden');
        document.getElementById('meaning-info').classList.add('hidden');
        
        const koBox = document.getElementById('ko-box');
        koBox.style.cursor = 'pointer';
        koBox.style.pointerEvents = 'auto';

        const q = currentQuestions[currentIndex];
        document.getElementById('question-counter').innerText = `문제 ${currentIndex + 1} / ${currentQuestions.length}`;
        document.getElementById('ko-text').innerText = q.ko;
        document.getElementById('en-text').innerText = q.en;
        document.getElementById('source-info').innerText = `출처: ${q.source}`;
        
        if(isPhrasalMode && q.meaning) {
            document.getElementById('meaning-info').innerText = q.meaning;
        }

        const starBtn = document.getElementById('btn-star');
        if (starredQuestions.includes(q)) {
            starBtn.classList.add('active');
            starBtn.innerText = "⭐ 어려워요 (저장됨)";
        } else {
            starBtn.classList.remove('active');
            starBtn.innerText = "⭐ 어려워요";
        }
    }

    function showAnswer() {
        const koBox = document.getElementById('ko-box');
        koBox.style.cursor = 'default';
        koBox.style.pointerEvents = 'none';

        document.getElementById('answer-section').classList.remove('hidden');
        
        if(isPhrasalMode && currentQuestions[currentIndex].meaning) {
            document.getElementById('meaning-info').classList.remove('hidden');
        }
        
        if (currentIndex < currentQuestions.length - 1) {
            document.getElementById('btn-next').classList.remove('hidden');
        } else {
            document.getElementById('btn-finish').classList.remove('hidden');
        }
    }

    function toggleStar() {
        const q = currentQuestions[currentIndex];
        const starBtn = document.getElementById('btn-star');
        const index = starredQuestions.indexOf(q);
        
        if (index > -1) {
            starredQuestions.splice(index, 1);
            starBtn.classList.remove('active');
            starBtn.innerText = "⭐ 어려워요";
        } else {
            starredQuestions.push(q);
            starBtn.classList.add('active');
            starBtn.innerText = "⭐ 어려워요 (저장됨)";
        }
    }

    function playTTS() {
        const textToSpeak = currentQuestions[currentIndex].en;
        if ('speechSynthesis' in window) {
            window.speechSynthesis.cancel();
            const utterance = new SpeechSynthesisUtterance(textToSpeak);
            utterance.lang = 'en-US';
            utterance.rate = 0.9; 
            window.speechSynthesis.speak(utterance);
        } else {
            alert('이 브라우저에서는 음성 듣기 기능을 지원하지 않습니다.');
        }
    }

    function nextQuestion() {
        currentIndex++;
        loadQuestion();
    }

    function showReview() {
        document.getElementById('quiz-area').classList.add('hidden');
        document.getElementById('review-area').classList.remove('hidden');
        document.getElementById('main-title').innerText = "결과 및 오답 노트";

        const reviewList = document.getElementById('review-list');
        reviewList.innerHTML = '';

        if (starredQuestions.length === 0) {
            reviewList.innerHTML = `<div class="review-empty">🎉 어려운 문장이 없습니다! 완벽해요! 🎉</div>`;
        } else {
            starredQuestions.forEach((q, idx) => {
                let meaningHtml = (isPhrasalMode && q.meaning) ? `<div style="font-size: 13px; color: #b45309; background: #fef3c7; padding: 4px 8px; border-radius: 4px; display: inline-block; margin-bottom: 5px;">${q.meaning}</div>` : '';
                
                reviewList.innerHTML += `
                    <div class="review-card">
                        <div style="font-size: 12px; color: #94a3b8; margin-bottom: 5px;">${idx + 1}. 출처: ${q.source}</div>
                        ${meaningHtml}
                        <div class="review-ko">${q.ko}</div>
                        <div class="review-en">${q.en}</div>
                    </div>
                `;
            });
        }
    }

    function resetToHome() {
        document.getElementById('review-area').classList.add('hidden');
        document.getElementById('mode-selection').classList.remove('hidden');
        document.getElementById('main-title').innerText = "🚀 스피드 영어 퀴즈";
    }
</script>

</body>
</html>
