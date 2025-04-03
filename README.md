<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>스마트 신발: 자동 굽 조절 시스템 대시보드</title>
  <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.4/css/all.min.css" rel="stylesheet">
  <link href="https://cdnjs.cloudflare.com/ajax/libs/tailwindcss/2.2.19/tailwind.min.css" rel="stylesheet">
  <script src="https://cdnjs.cloudflare.com/ajax/libs/recharts/2.1.9/Recharts.js"></script>
  <style>
    .animate-pulse {
      animation: pulse 2s cubic-bezier(0.4, 0, 0.6, 1) infinite;
    }
    .animate-ping {
      animation: ping 1s cubic-bezier(0, 0, 0.2, 1) infinite;
    }
    @keyframes pulse {
      0%, 100% {
        opacity: 1;
      }
      50% {
        opacity: .5;
      }
    }
    @keyframes ping {
      75%, 100% {
        transform: scale(2);
        opacity: 0;
      }
    }
  </style>
</head>
<body class="bg-slate-50 min-h-screen font-sans text-slate-800">
  <!-- 헤더 -->
  <header class="bg-gradient-to-r from-blue-600 to-blue-800 text-white p-6 text-center shadow-lg relative overflow-hidden">
    <div class="relative z-10">
      <h1 class="text-2xl font-bold flex items-center justify-center">
        <i class="fas fa-shoe-prints mr-2"></i> 스마트 신발: 자동 굽 조절 시스템 대시보드
      </h1>
    </div>
  </header>
  
  <div class="max-w-6xl mx-auto p-6">
    <!-- 시스템 상태 카드 -->
    <div class="bg-white p-6 rounded-xl shadow-md mb-6 border border-slate-100">
      <h2 class="text-xl text-blue-600 font-semibold pb-4 border-b border-slate-100 mb-4">
        <i class="fas fa-tachometer-alt mr-2"></i> 시스템 상태
      </h2>
      <div class="grid grid-cols-1 md:grid-cols-4 gap-4">
        <div class="bg-slate-50 p-4 rounded-lg text-center">
          <div class="text-sm text-slate-500 font-medium">라즈베리파이 상태</div>
          <div class="text-lg font-bold text-slate-800 mt-2 flex items-center justify-center">
            <span class="inline-block w-3 h-3 rounded-full mr-2 bg-green-500 animate-pulse"></span>온라인
          </div>
        </div>
        <div class="bg-slate-50 p-4 rounded-lg text-center">
          <div class="text-sm text-slate-500 font-medium">배터리</div>
          <div class="text-lg font-bold text-slate-800 mt-2 flex items-center justify-center">
            <div class="relative w-10 h-5 bg-slate-200 rounded mr-2 overflow-hidden">
              <div class="absolute left-0 top-0 h-full w-4/5 bg-gradient-to-r from-green-500 to-green-400"></div>
            </div>
            85%
          </div>
        </div>
        <div class="bg-slate-50 p-4 rounded-lg text-center">
          <div class="text-sm text-slate-500 font-medium">자이로스코프</div>
          <div class="text-lg font-bold text-slate-800 mt-2 flex items-center justify-center">
            <span class="inline-block w-3 h-3 rounded-full mr-2 bg-green-500 animate-pulse"></span>연결됨
          </div>
        </div>
        <div class="bg-slate-50 p-4 rounded-lg text-center">
          <div class="text-sm text-slate-500 font-medium">GPS</div>
          <div class="text-lg font-bold text-slate-800 mt-2 flex items-center justify-center">
            <span class="inline-block w-3 h-3 rounded-full mr-2 bg-green-500 animate-pulse"></span>연결됨
          </div>
        </div>
      </div>
    </div>
    
    <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
      <!-- 실시간 경사도 카드 -->
      <div class="bg-white p-6 rounded-xl shadow-md border border-slate-100">
        <h2 class="text-xl text-blue-600 font-semibold pb-4 border-b border-slate-100 mb-4">
          <i class="fas fa-ruler-vertical mr-2"></i> 실시간 경사도 및 굽 조절
        </h2>
        
        <div class="grid grid-cols-2 gap-4 mb-6">
          <div class="bg-slate-50 p-4 rounded-lg text-center">
            <div class="text-sm text-slate-500 font-medium">현재 경사각</div>
            <div class="text-xl font-bold text-blue-600 mt-2" id="currentAngle">5.2°</div>
          </div>
          <div class="bg-slate-50 p-4 rounded-lg text-center">
            <div class="text-sm text-slate-500 font-medium">굽 높이</div>
            <div class="text-xl font-bold text-blue-600 mt-2" id="heelHeight">2.5cm</div>
          </div>
        </div>
        
        <!-- 신발 굽 시각화 -->
        <div class="flex justify-center my-6">
          <div class="relative w-40 h-24 flex justify-center items-center">
            <div class="absolute bottom-0 w-60 h-4 bg-gradient-to-r from-slate-400 to-slate-300 rounded-2xl"></div>
            <div class="absolute w-56 h-20 bg-gradient-to-r from-slate-700 to-slate-600 rounded-2xl"></div>
            <div 
              class="absolute bottom-0 right-8 w-10 h-16 bg-gradient-to-b from-blue-600 to-blue-700 rounded-t-md shadow-lg transition-transform duration-300"
              id="heelVisual"
              style="transform: rotate(5.2deg); transform-origin: bottom"
            >
              <div class="absolute -top-6 left-1/2 transform -translate-x-1/2 bg-slate-800 text-white text-xs px-2 py-1 rounded" id="angleDisplay">
                5.2°
              </div>
            </div>
          </div>
        </div>
        
        <!-- 컨트롤 버튼 -->
        <div class="flex flex-wrap gap-2 mt-6">
          <button 
            id="increaseAngle"
            class="bg-blue-600 text-white px-4 py-2 rounded-lg shadow hover:bg-blue-700 transition-colors flex items-center"
          >
            <i class="fas fa-chevron-up mr-2"></i> 굽 높이 증가
          </button>
          <button 
            id="decreaseAngle"
            class="bg-blue-600 text-white px-4 py-2 rounded-lg shadow hover:bg-blue-700 transition-colors flex items-center"
          >
            <i class="fas fa-chevron-down mr-2"></i> 굽 높이 감소
          </button>
          <button 
            id="toggleAutoMode"
            class="bg-green-600 hover:bg-green-700 text-white px-4 py-2 rounded-lg shadow transition-colors flex items-center"
          >
            <i class="fas fa-magic mr-2"></i> 자동 모드 (켜짐)
          </button>
        </div>
      </div>
      
      <!-- 보행 패턴 분석 카드 -->
      <div class="bg-white p-6 rounded-xl shadow-md border border-slate-100">
        <h2 class="text-xl text-blue-600 font-semibold pb-4 border-b border-slate-100 mb-4">
          <i class="fas fa-walking mr-2"></i> 보행 패턴 분석
        </h2>
        
        <!-- 보행 차트 -->
        <div class="h-64 bg-slate-50 rounded-lg p-2 mb-4" id="walkingChart">
          <!-- Chart will be rendered here by JavaScript -->
          <div class="flex items-center justify-center h-full text-slate-400">
            <i class="fas fa-chart-line mr-2"></i> 차트 로딩 중...
          </div>
        </div>
        
        <div class="grid grid-cols-3 gap-4">
          <div class="bg-slate-50 p-4 rounded-lg text-center">
            <div class="text-sm text-slate-500 font-medium">보폭 수</div>
            <div class="text-xl font-bold text-blue-600 mt-2">1,248</div>
          </div>
          <div class="bg-slate-50 p-4 rounded-lg text-center">
            <div class="text-sm text-slate-500 font-medium">평균 보폭</div>
            <div class="text-xl font-bold text-blue-600 mt-2">73cm</div>
          </div>
          <div class="bg-slate-50 p-4 rounded-lg text-center">
            <div class="text-sm text-slate-500 font-medium">칼로리</div>
            <div class="text-xl font-bold text-blue-600 mt-2">287</div>
          </div>
        </div>
      </div>
      
      <!-- 지형 및 GPS 정보 카드 -->
      <div class="bg-white p-6 rounded-xl shadow-md border border-slate-100">
        <h2 class="text-xl text-blue-600 font-semibold pb-4 border-b border-slate-100 mb-4">
          <i class="fas fa-map-marked-alt mr-2"></i> 지형 및 GPS 정보
        </h2>
        
        <!-- GPS 지도 시각화 -->
        <div class="h-64 bg-gradient-to-br from-slate-200 to-slate-300 rounded-lg overflow-hidden relative mb-4 flex items-center justify-center">
          <div class="absolute inset-0 bg-blue-100 opacity-30 z-0"></div>
          <div class="grid grid-cols-8 grid-rows-6 w-full h-full z-0">
            <!-- Grid cells for map -->
            <div class="border-slate-300 border-[0.5px] opacity-30"></div>
            <div class="border-slate-300 border-[0.5px] opacity-30"></div>
            <!-- Repeated for all grid cells, simplified for brevity -->
          </div>
          <div class="absolute top-1/4 left-1/4 w-6 h-6 bg-blue-600 rounded-full shadow-lg animate-pulse z-10"></div>
          <div class="absolute top-1/4 left-1/4 w-16 h-16 bg-blue-600 rounded-full opacity-20 animate-ping"></div>
          <div class="absolute top-1/2 left-1/2 w-10 h-10 bg-red-600 rounded-full shadow-lg z-10 transform -translate-x-1/2 -translate-y-1/2"></div>
          
          <div class="z-20 text-center">
            <i class="fas fa-map-marker-alt text-3xl text-red-600 mb-2"></i>
            <p class="font-medium text-slate-800 bg-white bg-opacity-70 px-4 py-2 rounded">목적지까지 14분</p>
          </div>
          
          <!-- 경로 시뮬레이션 -->
          <div class="absolute top-0 left-0 w-full h-full z-10 pointer-events-none">
            <svg width="100%" height="100%" stroke-linecap="round">
              <path 
                d="M80,80 C120,150 200,100 300,140 S350,180 400,220" 
                fill="none" 
                stroke="#3b82f6" 
                stroke-width="4" 
                stroke-dasharray="10,5"
              />
            </svg>
          </div>
        </div>
        
        <div class="grid grid-cols-3 gap-4">
          <div class="bg-slate-50 p-4 rounded-lg text-center">
            <div class="text-sm text-slate-500 font-medium">평균 경사도</div>
            <div class="text-xl font-bold text-blue-600 mt-2">3.8°</div>
          </div>
          <div class="bg-slate-50 p-4 rounded-lg text-center">
            <div class="text-sm text-slate-500 font-medium">이동 거리</div>
            <div class="text-xl font-bold text-blue-600 mt-2">2.4km</div>
          </div>
          <div class="bg-slate-50 p-4 rounded-lg text-center">
            <div class="text-sm text-slate-500 font-medium">예상 도착</div>
            <div class="text-xl font-bold text-blue-600 mt-2">14분</div>
          </div>
        </div>
      </div>
      
      <!-- 시스템 알림 카드 -->
      <div class="bg-white p-6 rounded-xl shadow-md border border-slate-100">
        <h2 class="text-xl text-blue-600 font-semibold pb-4 border-b border-slate-100 mb-4">
          <i class="fas fa-bell mr-2"></i> 시스템 알림
        </h2>
        
        <div class="max-h-64 overflow-y-auto pr-2" id="notificationsContainer">
          <!-- Notification items will be added by JavaScript -->
          <div class="p-4 mb-3 rounded-lg border-l-4 bg-green-50 border-green-500 transition-transform hover:translate-x-1">
            <div class="flex items-start">
              <div class="text-xl mr-3 text-green-500">
                <i class="fas fa-check-circle"></i>
              </div>
              <div>
                <strong class="font-medium">10:30</strong> - 스마트 신발 시스템이 시작되었습니다.
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    
    <!-- 모터 및 센서 상태 테이블 -->
    <div class="bg-white p-6 rounded-xl shadow-md mt-6 border border-slate-100">
      <h2 class="text-xl text-blue-600 font-semibold pb-4 border-b border-slate-100 mb-4">
        <i class="fas fa-microchip mr-2"></i> 모터 및 센서 상태
      </h2>
      
      <div class="overflow-x-auto">
        <table class="min-w-full border-collapse">
          <thead>
            <tr class="bg-slate-50">
              <th class="py-4 px-6 text-left font-semibold text-slate-700">
                <i class="fas fa-cog mr-2"></i> 구성 요소
              </th>
              <th class="py-4 px-6 text-left font-semibold text-slate-700">
                <i class="fas fa-heartbeat mr-2"></i> 상태
              </th>
              <th class="py-4 px-6 text-left font-semibold text-slate-700">
                <i class="fas fa-chart-bar mr-2"></i> 값
              </th>
              <th class="py-4 px-6 text-left font-semibold text-slate-700">
                <i class="fas fa-battery-half mr-2"></i> 배터리 소모
              </th>
            </tr>
          </thead>
          <tbody>
            <tr class="hover:bg-slate-50">
              <td class="py-4 px-6 border-t border-slate-100">MPU-6050 센서</td>
              <td class="py-4 px-6 border-t border-slate-100">
                <span class="inline-block w-3 h-3 rounded-full mr-2 bg-green-500 animate-pulse"></span> 정상
              </td>
              <td class="py-4 px-6 border-t border-slate-100">X: 0.2, Y: 5.2, Z: 0.1</td>
              <td class="py-4 px-6 border-t border-slate-100">낮음</td>
            </tr>
            <tr class="hover:bg-slate-50">
              <td class="py-4 px-6 border-t border-slate-100">서보모터 (MG996R)</td>
              <td class="py-4 px-6 border-t border-slate-100">
                <span class="inline-block w-3 h-3 rounded-full mr-2 bg-green-500 animate-pulse"></span> 정상
              </td>
              <td class="py-4 px-6 border-t border-slate-100">PWM: 1500μs</td>
              <td class="py-4 px-6 border-t border-slate-100">중간</td>
            </tr>
            <tr class="hover:bg-slate-50">
              <td class="py-4 px-6 border-t border-slate-100">압력 센서</td>
              <td class="py-4 px-6 border-t border-slate-100">
                <span class="inline-block w-3 h-3 rounded-full mr-2 bg-green-500 animate-pulse"></span> 정상
              </td>
              <td class="py-4 px-6 border-t border-slate-100">75kg</td>
              <td class="py-4 px-6 border-t border-slate-100">낮음</td>
            </tr>
            <tr class="hover:bg-slate-50">
              <td class="py-4 px-6 border-t border-slate-100">Raspberry Pi Zero 2 W</td>
              <td class="py-4 px-6 border-t border-slate-100">
                <span class="inline-block w-3 h-3 rounded-full mr-2 bg-green-500 animate-pulse"></span> 정상
              </td>
              <td class="py-4 px-6 border-t border-slate-100">CPU: 15%, Temp: 42°C</td>
              <td class="py-4 px-6 border-t border-slate-100">중간</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>

  <script>
    // 시뮬레이션 데이터 생성 및 상태 관리
    let autoMode = true;
    let currentAngle = 5.2;
    let notifications = [];
    let walkingData = [];
    
    // 초기화 함수
    function init() {
      // 걸음 데이터 생성
      walkingData = [];
      for (let i = 0; i < 12; i++) {
        walkingData.push({
          time: `${i * 5}분`,
          걸음수: Math.floor(Math.random() * 100) + 80,
          경사도: Math.floor(Math.random() * 8) + 1
        });
      }
      
      // 초기 알림 추가
      addNotification('스마트 신발 시스템이 시작되었습니다.', 'success');
      
      // 이벤트 리스너 설정
      document.getElementById('increaseAngle').addEventListener('click', () => adjustHeel('up'));
      document.getElementById('decreaseAngle').addEventListener('click', () => adjustHeel('down'));
      document.getElementById('toggleAutoMode').addEventListener('click', toggleAutoMode);
      
      // 자동 모드 시뮬레이션 시작
      if (autoMode) {
        startAutoModeSimulation();
      }
    }
    
    // 굽 높이 조절 함수
    function adjustHeel(direction) {
      let newAngle = currentAngle;
      if (direction === 'up') {
        newAngle += 1;
        if (newAngle > 15) newAngle = 15;
      } else {
        newAngle -= 1;
        if (newAngle < 0) newAngle = 0;
      }
      
      currentAngle = newAngle;
      updateHeelDisplay();
      addNotification(`굽 높이 수동 조절: ${newAngle.toFixed(1)}°`, 'normal');
    }
    
    // 자동 모드 토글 함수
    function toggleAutoMode() {
      autoMode = !autoMode;
      const autoModeBtn = document.getElementById('toggleAutoMode');
      
      if (autoMode) {
        autoModeBtn.classList.remove('bg-slate-500', 'hover:bg-slate-600');
        autoModeBtn.classList.add('bg-green-600', 'hover:bg-green-700');
        autoModeBtn.innerHTML = '<i class="fas fa-magic mr-2"></i> 자동 모드 (켜짐)';
        addNotification('자동 모드 활성화됨', 'success');
        startAutoModeSimulation();
      } else {
        autoModeBtn.classList.remove('bg-green-600', 'hover:bg-green-700');
        autoModeBtn.classList.add('bg-slate-500', 'hover:bg-slate-600');
        autoModeBtn.innerHTML = '<i class="fas fa-magic mr-2"></i> 자동 모드 (꺼짐)';
        addNotification('자동 모드 비활성화됨', 'warning');
        stopAutoModeSimulation();
      }
    }
    
    // 알림 추가 함수
    function addNotification(message, type = 'normal') {
      const now = new Date();
      const hours = now.getHours();
      const minutes = now.getMinutes().toString().padStart(2, '0');
      const time = `${hours}:${minutes}`;
      
      const notification = {
        id: Date.now(),
        time,
        message,
        type
      };
      
      notifications = [notification, ...notifications.slice(0, 6)];
      updateNotificationsDisplay();
    }
    
    // 굽 높이 디스플레이 업데이트
    function updateHeelDisplay() {
      const angleElement = document.getElementById('currentAngle');
      const heightElement = document.getElementById('heelHeight');
      const heelVisual = document.getElementById('heelVisual');
      const angleDisplay = document.getElementById('angleDisplay');
      
      // 굽 높이 계산
      const heelHeight = (2 + (currentAngle / 10)).toFixed(1);
      
      angleElement.textContent = `${currentAngle.toFixed(1)}°`;
      heightElement.textContent = `${heelHeight}cm`;
      heelVisual.style.transform = `rotate(${currentAngle}deg)`;
      angleDisplay.textContent = `${currentAngle.toFixed(1)}°`;
    }
    
    // 알림 디스플레이 업데이트
    function updateNotificationsDisplay() {
      const container = document.getElementById('notificationsContainer');
      container.innerHTML = '';
      
      notifications.forEach(notification => {
        const className = notification.type === 'success' 
          ? 'bg-green-50 border-green-500 text-green-500'
          : notification.type === 'warning'
            ? 'bg-orange-50 border-orange-500 text-orange-500'
            : 'bg-slate-50 border-slate-400 text-slate-500';
        
        const iconClass = notification.type === 'success'
          ? 'fa-check-circle'
          : notification.type === 'warning'
            ? 'fa-exclamation-triangle'
            : 'fa-info-circle';
        
        const html = `
          <div class="p-4 mb-3 rounded-lg border-l-4 ${notification.type === 'success' ? 'bg-green-50 border-green-500' : notification.type === 'warning' ? 'bg-orange-50 border-orange-500' : 'bg-slate-50 border-slate-400'} transition-transform hover:translate-x-1">
            <div class="flex items-start">
              <div class="${notification.type === 'success' ? 'text-green-500' : notification.type === 'warning' ? 'text-orange-500' : 'text-slate-500'} text-xl mr-3">
                <i class="fas ${iconClass}"></i>
              </div>
              <div>
                <strong class="font-medium">${notification.time}</strong> - ${notification.message}
              </div>
            </div>
          </div>
        `;
        
        container.innerHTML += html;
      });
    }
    
    // 자동 모드 시뮬레이션 변수
    let autoModeInterval;
    
    // 자동 모드 시뮬레이션 시작
    function startAutoModeSimulation() {
      if (autoModeInterval) clearInterval(autoModeInterval);
      
      autoModeInterval = setInterval(() => {
        if (!autoMode) return;
        
        const randomChange = (Math.random() - 0.5) * 2;
        let newAngle = currentAngle + randomChange;
        
        // 범위 제한
        if (newAngle < 0) newAngle = 0;
        if (newAngle > 15) newAngle = 15;
        
        currentAngle = newAngle;
        updateHeelDisplay();
        
        // 상당한 변화가 있을 때만 알림 추가
        if (Math.abs(randomChange) > 1) {
          const newHeight = (2 + (newAngle / 10)).toFixed(1);
          addNotification(`경사 변화 감지: 굽 높이 자동 조정 (${newHeight}cm)`, 'success');
        }
      }, 5000);
    }
    
    // 자동 모드 시뮬레이션 중지
    function stopAutoModeSimulation() {
      if (autoModeInterval) {
        clearInterval(autoModeInterval);
        autoModeInterval = null;
      }
    }
    
    // 페이지 로드 시 초기화
    window.addEventListener('DOMContentLoaded', init);
  </script>
</body>
</html>
