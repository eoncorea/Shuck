<html lang="ko">
<head>
  <meta charset="UTF-8">
  <title>Shuck슈크 시그목록</title>
  <style>
    body {
      font-family: 'Segoe UI', sans-serif;
      padding: 40px;
      background: linear-gradient(to right, #fdfbfb, #ebedee);
      color: #111;
    }
    img.logo {
      width: 220px;
      display: block;
      margin: 0 auto 30px auto;
      border-radius: 10px;
      box-shadow: 0 4px 12px rgba(0,0,0,0.1);
    }
    h1 {
      text-align: center;
      font-size: 30px;
      color: #333;
      margin-bottom: 10px;
    }
    input[type="text"] {
      display: block;
      margin: 20px auto;
      padding: 10px;
      font-size: 16px;
      width: 320px;
      border: 1px solid #bbb;
      border-radius: 8px;
      box-shadow: inset 0 1px 3px rgba(0,0,0,0.1);
    }
    .table-container {
      height: 500px;
      overflow-y: scroll;
      margin: 0 auto;
      max-width: 520px;
      border-radius: 8px;
      box-shadow: 0 2px 8px rgba(0,0,0,0.05);
      background: white;
    }
    table {
      width: 100%;
      border-collapse: collapse;
    }
    th, td {
      text-align: left;
      padding: 12px 15px;
      border-bottom: 1px solid #eee;
    }
    th {
      background: #f6f6f6;
      cursor: pointer;
      position: sticky;
      top: 0;
      z-index: 1;
    }
    th.sortable:hover {
      background-color: #eaeaea;
    }
    td.price {
      width: 80px;
      font-weight: bold;
      color: #444;
    }
    .count {
      text-align: center;
      margin-top: 20px;
      font-size: 14px;
      color: #666;
    }
  </style>
</head>
<body>
  <img src="https://stimg.sooplive.co.kr/NORMAL_BBS/8/19611068/4621686a578f4719a.jpeg" class="logo" alt="Shuck Logo">
  <h1>Shuck 시그목록</h1>
  <input type="text" id="searchInput" placeholder="곡 제목으로로 검색 (예:뛰어)">
  <div class="table-container">
    <table id="sigTable">
      <thead>
        <tr>
          <th class="sortable" id="sortPrice">번호 ▼</th>
          <th>제목</th>
        </tr>
      </thead>
      <tbody></tbody>
    </table>
  </div>
  <div class="count" id="countInfo"></div>

  <script>
    const sigData = `88 💜 1999(힙레)
92 💜 간체조
93 💜 나니가스키
94 💜 쓸애기
95 💜 릴리라잌유
96 💜 섹시걸
97 💜 괜찮아 딩딩딩
98 💜 오빠
99 💜 삐삐
100 💝 시그 룰렛
140 💝 먹어
150 💝 먹지마
160 💝 먹먹마 룰렛
162 💝 신청댄스
100 💝 시그 룰렛
101 ❤️ 핫해
102 ❤️ 나랑사귈래
103 ❤️ LALALA
104 ❤️ 버터플라이
105 ❤️ 엉덩이
106 ❤️ 슈퍼소닉
107 ❤️ 이글루
108 ❤️ 인민댄스
109 ❤️ Fast car
110 🧡 꽃
111 🧡 외모 췤
112 🧡 캠퍼
113 🧡 빛을따라서
114 🧡 Love Boom
115 🧡 Hold UP
116 🧡 왜요왜요
117 🧡 Chili
118 🧡 우치다
119 🧡 Just Blow(경찰춤)
120 💛 슈쿠란보
121 💛 Bubble
122 💛 그래비티
123 💛 푸른산호초
124 💛 핑크레이디
125 💛 러브쉐이크
126 💛 하입보이
127 💛 Smoke
128 💛 나루토
129 💛 어마어마해
130 💚 너와의 모든 지금
131 💚 Fast Forward
132 💚 𝐋𝐒𝐃
133 💚 스파클링
134 💚 전야
135 💚 킥드럼베이스
136 💚 테디베어
137 💚 클락션
138 💚 옴브리뉴
139 💚 댄싱퀸
141 💙 Hentai
142 💙 아마겟돈
143 💙 슈퍼노바
144 💙 헤이마마
145 💙 How Sweet
146 💙 Sticky
147 💙 아파트
148 💙 Drama
149 💙 사이렌(선미)
151 💙 Whiplash
152 🤍 칙칙붐
153 🤍 웨이백홈
154 🤍 고민중독
155 🤍 간바레
156 🤍 Balloon in love
157 🤍 라잌제니
158 🤍 UP
159 🤍 붐붐베이스
161 🤍 청바지
162 ❤️ 신청댄스
163 🖤 마티니
164 🖤 오늘밤새
165 🖤 핸즈업
166 🖤 천사
167 🖤 이쁜게죄
168 🖤 HOT
201 🖤 터미널
202 🖤 썸머필링
203 🖤 야메떼
204 🖤 홈스윗홈
205 🖤 싱글레이디
206 🖤 우유
207 🖤 아이돌(R-MIX)
208 🖤 계엄령
256 🖤 아기상어
257 🖤 사이렌(라이즈)
357 🖤 빠우치
452 🖤 슈퍼그래비티
500 🖤 개포동의눈물`;

    const sigList = sigData.trim().split('\n').map(line => {
      const [price, ...titleParts] = line.trim().split(' ');
      return { price: parseInt(price), title: titleParts.join(' ') };
    });

    const tableBody = document.querySelector('#sigTable tbody');
    const countInfo = document.getElementById('countInfo');

    function renderTable(data) {
      tableBody.innerHTML = data.map(sig => `
        <tr>
          <td class="price">${sig.price}</td>
          <td>${sig.title}</td>
        </tr>
      `).join('');
      countInfo.textContent = `총 ${data.length.toLocaleString()}개의 곡이 있습니다.`;
    }

    document.getElementById('searchInput').addEventListener('input', e => {
      const keyword = e.target.value.toLowerCase();
      const filtered = sigList.filter(sig => sig.title.toLowerCase().includes(keyword));
      renderTable(filtered);
    });

    let ascending = true;
    document.getElementById('sortPrice').addEventListener('click', () => {
      ascending = !ascending;
      const sorted = [...sigList].sort((a, b) => ascending ? a.price - b.price : b.price - a.price);
      renderTable(sorted);
      document.getElementById('sortPrice').textContent = ascending ? '번호 ▲' : '번호 ▼';
    });

    renderTable(sigList);
  </script>
</body>
</html>
