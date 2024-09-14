<template>
  <div class="min-h-screen bg-app bg-center bg-no-repeat bg-cover ">
    <div class="text-center pt-4">
      <h2 class="text-3xl text-white font-semibold">Laundromat Company</h2>
      <p class="mt-6 text-white ">สะอาด ปลอดภัย ไร้กังวล ด้วยเทคโนโลยีการฆ่าเชื้อโรคขั้นสูง
        มั่นใจได้เลยว่าผ้าที่ซักจากร้านของเรานั้นสะอาด ปลอดภัย ไร้กลิ่นอับ พร้อมสวมใส่อย่างมั่นใจ </p>
    </div>
    <div class="flex  justify-center items-center ">

      <main
        class="flex flex-col lg:flex-row bg-white backdrop-filter backdrop-blur-lg bg-opacity-20 rounded-xl overflow-hidden w-full max-w-5xl shadow-lg m-4 lg:m-6">
        <div class="flex-1 p-4 lg:p-6">
          <div class="text-lg text-white mb-8 flex items-center">
            <svg class="w-6 h-6" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg">
              <path fill-rule="evenodd"
                d="M4.083 9h1.946c.089-1.546.383-2.97.837-4.118A6.004 6.004 0 004.083 9zM10 2a8 8 0 100 16 8 8 0 000-16zm0 2c-.076 0-.232.032-.465.262-.238.234-.497.623-.737 1.182-.389.907-.673 2.142-.766 3.556h3.936c-.093-1.414-.377-2.649-.766-3.556-.24-.56-.5-.948-.737-1.182C10.232 4.032 10.076 4 10 4zm3.971 5c-.089-1.546-.383-2.97-.837-4.118A6.004 6.004 0 0115.917 9h-1.946zm-2.003 2H8.032c.093 1.414.377 2.649.766 3.556.24.56.5.948.737 1.182.233.23.389.262.465.262.076 0 .232-.032.465-.262.238-.234.498-.623.737-1.182.389-.907.673-2.142.766-3.556zm1.166 4.118c.454-1.147.748-2.572.837-4.118h1.946a6.004 6.004 0 01-2.783 4.118zm-6.268 0C6.412 13.97 6.118 12.546 6.03 11H4.083a6.004 6.004 0 002.783 4.118z"
                clip-rule="evenodd"></path>
            </svg>
            <div class="ml-4 font-bold">เครื่องซักผ้าทั้งหมด</div>
          </div>
          <div class="grid mt-8  gap-8 grid-cols-1 md:grid-cols-2 xl:grid-cols-2">
            <div v-for="machine in machines" :key="machine.id" class="flex flex-col">
              <div class="bg-white shadow-md  rounded-3xl p-4">
                <div class="flex-none lg:flex">
                  <div class=" h-full w-full lg:h-48 lg:w-48   lg:mb-0 mb-3">
                    <img :src="machine.image" :alt="machine.name" alt="Just a flower"
                      class=" w-full  object-scale-down lg:object-cover  lg:h-48 rounded-2xl">
                  </div>
                  <div class="flex-auto ml-3 justify-evenly py-2">
                    <div class="flex flex-wrap ">
                      <div class="w-full flex-none text-xs text-blue-700 font-medium ">
                        Laundromat
                      </div>
                      <h2 class="flex-auto text-lg font-medium">{{ machine.name }}</h2>
                    </div>
                    <span class="mt-3"></span>
                    <div class="flex py-2  text-sm text-gray-500">

                      <div class="flex-1 inline-flex items-center text-[#800000]">
                        <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-2 " fill="none" viewBox="0 0 24 24"
                          stroke="currentColor">
                          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                            d="M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z"></path>
                        </svg>
                        <span class="font-semibold  text-xl ">{{ Math.floor(machine.timeRemaining / 60) }}m {{
                          machine.timeRemaining % 60 }}s</span>
                      </div>
                    </div>
                    <div class="flex p-4 pb-2 border-t border-gray-200 "></div>
                    <div class="flex space-x-3 text-sm font-medium">
                      <div class="flex-auto flex space-x-3">
                        <button class="mb-2 md:mb-0 bg-[#FEF9D9] px-4 py-2 shadow-sm tracking-wider border border-[#FCDE70] text-gray-600
                       rounded-full
                        inline-flex items-center space-x-2 ">
                          <span :class="machine.status === 'Available' ? 'text-[#379237]' : 'text-[#C40C0C]'">
                            <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 12 12">
                              <circle cx="6" cy="6" r="6" fill="currentColor" />
                            </svg>
                          </span>
                          <span :class="machine.status === 'Available' ? 'text-[#1A5319]' : 'text-[#800000]'">{{
                            machine.status === 'Available' ? 'ว่าง' : 'ใช้งานอยู่' }}</span>
                        </button>
                      </div>
                      <button @click="insertCoin(machine.id)" :disabled="machine.status !== 'Available'" type="button"
                        :class="machine.status === 'Available' ? 'bg-gray-900 hover:bg-gray-800' : 'bg-gray-400 cursor-not-allowed'"
                        class="mb-2 md:mb-0 px-5 py-2 shadow-sm tracking-wider text-white rounded-full">หยอดเหรียญ</button>
                    </div>

                  </div>
                </div>
              </div>
            </div>

          </div>
        </div>
      </main>
    </div>
  </div>

  <!-- <button @click="clearMachinesData">Clear Machine Data</button> -->

</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue';
import axios from 'axios';

const machines = ref([
  { id: 1, name: 'เครื่องที่ 1', image: '../../public/img/machine1.jpg', status: 'Available', timeRemaining: 0 },
  { id: 2, name: 'เครื่องที่ 2', image: '../../public/img/machine2.jpg', status: 'Available', timeRemaining: 0 },
  { id: 3, name: 'เครื่องที่ 3', image: '../../public/img/machine3.jpg', status: 'Available', timeRemaining: 0 },
  { id: 4, name: 'เครื่องที่ 4', image: '../../public/img/machine4.jpg', status: 'Available', timeRemaining: 0 },
  { id: 4, name: 'เครื่องที่ 5', image: '../../public/img/machine4.jpg', status: 'Available', timeRemaining: 0 },
  { id: 4, name: 'เครื่องที่ 6', image: '../../public/img/machine4.jpg', status: 'Available', timeRemaining: 0 },

]);

// ฟังก์ชันสำหรับเริ่มนับถอยหลัง
const startCountdown = (machine) => {
  const interval = setInterval(() => {
    if (machine.timeRemaining > 0) {
      machine.timeRemaining--;
    }
    if (machine.timeRemaining === 60) {
      console.log(`เครื่อง ${machine.id} เหลือเวลาน้อยกว่า 1 นาที`);
      sendLineNotification(`เครื่อง ${machine.name} เหลือเวลาใช้งานน้อยกว่า 1 นาที`);
    }
    if (machine.timeRemaining <= 0) {
      clearInterval(interval);
      machine.status = 'Available';
      machine.timeRemaining = 0;
    }
    saveMachines();
  }, 1000);

  machine.intervalId = interval;
};

// ฟังก์ชันสำหรับส่งการแจ้งเตือน
const sendLineNotification = async (message) => {
  const token = '4t6b39EK2bKiF4wbIXXhhV8NZxaINjSFJ8p9Uu3sCVF'; 

  try {
    const response = await fetch('https://notify-api.line.me/api/notify', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/x-www-form-urlencoded',
        'Authorization': `Bearer ${token}`
      },
      body: new URLSearchParams({
        message: message
      })
    });

    const data = await response.json();

    if (response.ok) {
      console.log('แจ้งเตือนสำเร็จ:', data);
    } else {
      console.error('การแจ้งเตือนล้มเหลว:', data);
    }
  } catch (error) {
    console.error('เกิดข้อผิดพลาด:', error);
  }
};
const insertCoin = (id) => {
  const machine = machines.value.find((m) => m.id === id);
  if (machine && machine.status === 'Available') {
    machine.status = 'In Use';
    machine.timeRemaining = 2 * 60;
    startCountdown(machine);
    saveMachines(); 
  }
};

const saveMachines = () => {
  const machinesInUse = machines.value.filter(machine => machine.status === 'In Use');

  if (machinesInUse.length > 0) {
    localStorage.setItem('machines', JSON.stringify(machines.value.map(machine => {
      const { intervalId, ...machineData } = machine;
      return machineData;
    })));
  } else {
    localStorage.removeItem('machines'); 
  }
};

const clearMachinesData = () => {
  localStorage.removeItem('machines');
  machines.value.forEach(machine => {
    machine.status = 'Available';
    machine.timeRemaining = 0;
  });
};

const loadMachines = () => {
  const savedMachines = localStorage.getItem('machines');
  if (savedMachines) {
    machines.value = JSON.parse(savedMachines);
    machines.value.forEach(machine => {
      if (machine.status === 'In Use' && machine.timeRemaining > 0) {
        startCountdown(machine);
      }
    });
  } else {
    machines.value.forEach(machine => {
      machine.status = 'Available';
      machine.timeRemaining = 0;
    });
  }
};


onMounted(() => {
  loadMachines();
});

// เพิ่มฟังก์ชันนี้เพื่อหยุดการนับถอยหลังเมื่อ component ถูกทำลาย
const stopAllCountdowns = () => {
  machines.value.forEach(machine => {
    if (machine.intervalId) {
      clearInterval(machine.intervalId);
    }
  });
};

onUnmounted(() => {
  stopAllCountdowns();
});
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Noto+Sans+JP:wght@300;400;500&display=swap');

* {
  font-family: "Kanit", sans-serif;
}

.bg-app {
  /* background-image: url('./img/pc.jpg'); */
  background-image: linear-gradient(120deg, #f093fb 0%, #f5576c 100%);
}
</style>