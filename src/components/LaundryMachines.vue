<!-- src/components/LaundryMachines.vue -->
<template>
  <div class="min-w-screen h-screen animated fadeIn faster  fixed  left-0 top-0 flex justify-center items-center inset-0 z-50 outline-none 
  focus:outline-none bg-no-repeat bg-center bg-cover" style="background-image: url(../../public/img/pc.jpg);"
    id="modal-id">
    <div class="absolute bg-black opacity-80 inset-0 z-0"></div>
    <div class="relative min-h-screen flex flex-col items-center justify-center ">
      <div class="text-center">
        <h2 class="text-3xl text-gray-950 dark:text-white font-semibold">Laundromat Company</h2>
        <p class="mt-6 text-gray-700 dark:text-gray-300">สะอาด ปลอดภัย ไร้กังวล ด้วยเทคโนโลยีการฆ่าเชื้อโรคขั้นสูง
          มั่นใจได้เลยว่าผ้าที่ซักจากร้านของเรานั้นสะอาด ปลอดภัย ไร้กลิ่นอับ พร้อมสวมใส่อย่างมั่นใจ </p>
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
                    <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-2 " fill="none"
                      viewBox="0 0 24 24" stroke="currentColor">
                      <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                        d="M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z"></path>
                    </svg>
                    <span class="font-semibold  text-xl ">{{ Math.floor(machine.timeRemaining / 60) }}m {{ machine.timeRemaining % 60 }}s</span>
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
                      <span :class="machine.status === 'Available' ? 'text-[#1A5319]' : 'text-[#800000]'">{{ machine.status === 'Available' ? 'ว่าง' : 'ใช้งานอยู่' }}</span>
                    </button>
                  </div>
                  <button @click="insertCoin(machine.id)" :disabled="machine.status !== 'Available'" type="button"
                    :class="machine.status === 'Available' ? 'bg-gray-900 hover:bg-gray-800' : 'bg-gray-400 cursor-not-allowed'"
                    class="mb-2 md:mb-0 px-5 py-2 shadow-sm tracking-wider text-white rounded-full">หยอดเหรียญ</button>
                </div>
                <!-- <button @click="clearMachinesData">Clear Machine Data</button> -->

              </div>
            </div>
          </div>
        </div>

      </div>
    </div>
  </div>

</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue';
import axios from 'axios';

const machines = ref([
  { id: 1, name: 'เครื่องที่ 1', image: '../../public/img/machine1.jpg', status: 'Available', timeRemaining: 0 },
  { id: 2, name: 'เครื่องที่ 2', image: '../../public/img/machine2.jpg', status: 'Available', timeRemaining: 0 },
  { id: 3, name: 'เครื่องที่ 3', image: '../../public/img/machine3.jpg', status: 'Available', timeRemaining: 0 },
  { id: 4, name: 'เครื่องที่ 4', image: '../../public/img/machine4.jpg', status: 'Available', timeRemaining: 0 },
]);

// ฟังก์ชันสำหรับเริ่มนับถอยหลัง
const startCountdown = (machine) => {
  const interval = setInterval(() => {
    if (machine.timeRemaining > 0) {
      machine.timeRemaining--;
    }
    if (machine.timeRemaining === 60) {
      console.log(`Machine ${machine.id} is almost done!`);

      // ส่งแจ้งเตือนไปยังกลุ่มไลน์เมื่อเวลาน้อยกว่า 1 นาที
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
const sendLineNotification = (message) => {
  const token = '4t6b39EK2bKiF4wbIXXhhV8NZxaINjSFJ8p9Uu3sCVF'; 

  axios.post('https://notify-api.line.me/api/notify', new URLSearchParams({
    message: message
  }), {
    headers: {
      'Content-Type': 'application/x-www-form-urlencoded',
      'Authorization': `Bearer ${token}`
    }
  })
  .then(response => {
    console.log('แจ้งเตือนสำเร็จ:', response.data);
  })
  .catch(error => {
    console.error('การแจ้งเตือนล้มเหลว:', error);
  });
};

const insertCoin = (id) => {
  const machine = machines.value.find((m) => m.id === id);
  if (machine && machine.status === 'Available') {
    machine.status = 'In Use';
    machine.timeRemaining = 2 * 60;
    startCountdown(machine);
    saveMachines(); // บันทึกเมื่อมีการหยอดเหรียญ
  }
};

const saveMachines = () => {
  // บันทึกเฉพาะเมื่อมีการเปลี่ยนแปลงสถานะหรือการใช้งานเครื่อง
  const machinesInUse = machines.value.filter(machine => machine.status === 'In Use');

  if (machinesInUse.length > 0) {
    localStorage.setItem('machines', JSON.stringify(machines.value.map(machine => {
      const { intervalId, ...machineData } = machine;
      return machineData;
    })));
  } else {
    localStorage.removeItem('machines'); // ลบข้อมูลออกจาก localStorage เมื่อไม่มีเครื่องที่ใช้งานอยู่
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
    // เริ่มการนับถอยหลังใหม่สำหรับเครื่องที่กำลังใช้งานอยู่
    machines.value.forEach(machine => {
      if (machine.status === 'In Use' && machine.timeRemaining > 0) {
        startCountdown(machine);
      }
    });
  } else {
    // ถ้าไม่มีข้อมูลใน localStorage ให้รีเซ็ตสถานะเครื่องทั้งหมด
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

// ใช้ onUnmounted hook เพื่อหยุดการนับถอยหลังเมื่อ component ถูกทำลาย
onUnmounted(() => {
  stopAllCountdowns();
});
</script>