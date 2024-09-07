## **Project Overview: Soil Moisture Detection System Using ESP32**

### **Objective:**

To create a soil moisture detection system that monitors soil moisture levels using a soil hygrometer humidity detection module. If the soil is dry (moisture below a threshold), the system will activate a motor (simulated with an LED) and sound a buzzer using a relay module, all controlled by an ESP32 microcontroller.

### **Components Required:**

1. **ESP32 Devkit V4** --[ESP32 ](https://www.amazon.in/SquadPixel-ESP-32-Bluetooth-Development-Board/dp/B071XP56LM?source=ps-sl-shoppingads-lpcontext&ref_=fplfs&psc=1&smid=AFNRVQ5JHRILL)
2. **Soil Hygrometer Humidity Detection Module (Moisture Sensor)** -- [SENSOR ](https://www.amazon.in/Electron-Moisture-Output-Soil-Hygrometer-Soil-Sensor-Soil/dp/B0D2BMSXQ8/ref=sr_1_6?dib=eyJ2IjoiMSJ9.2KMpMMNfMcGxlsHR5XLnhA8TeywVcvPUlSFOTZwLMyhYURmbhSjc_YDU8Lvs1IO5Bqs9YWm2z1QeiEDd7E575E1Tr7AUVJaRk-5eEYXNy77_dDLSHrKQW0qAqkRPgVT_6uuTA84muL1z_Tmxp0AAPQbm-d7VJGamgZ9a5yjAQgRHzDSs-YTmKaZmqXuXEHwWiPr2ctmTabgOLPip-MNP3RK95_5twX1w6RBbfwa8Oq_XnKJCNUHKrGylmYNvzCKuqNANjrYViBW4GbV_Xm-F_kO74NBzF_umij1nvIp7jyI.jbH20XKAp0W4ME8MDP8pLy22Z28VvxkQzfJpcbpjT5U&dib_tag=se&keywords=Moisture+Sensor&qid=1725280449&sr=8-6)
3. **5V One Channel Relay Module** --[Relay ](https://www.amazon.in/Techleads-Channel-Relay-Board-Arduino/dp/B085T457VD/ref=sr_1_3?dib=eyJ2IjoiMSJ9.Du2-hrZCxhda1Y3asdohcQTydANi42k4LTUDJJO8Gz1phRhBS5jUxqCN885Gg-n8op6tMPx3IaUGeR_VjKCazdkgidlDm-vBp0GrrPD0L9KIBkWHpjmMkHms6LSE5Fy1loA7sjDg2xnYL1ETLfmTiA_FPXXB_I_KE3xvs3IYwtefvQSWr_5ueRwIGFmXKnoIljxO0OgWJ3rzGo3YvvpUNqGJU9hsvemHQS1Gr79OCEyBmYMBziUyRc3uz05ntSlIVpWrcuqE1uAxsDGV3fEqh33N-_-TR_1d7VwPcu3Buxo.djyHQcJVnnEayrzyS960_ui44E2TWfizojlhsHuHpyM&dib_tag=se&keywords=5V+Relay&qid=1725285174&sr=8-3)
4. **LED (to simulate a motor)**
5. **3-24V Small Enclosed Piezo Electronic Buzzer** --[Buzzer](https://www.amazon.in/Buzzer-Small-Enclosed-Piezo-Electronic/dp/B07P84JGLB/ref=pd_sbs_d_sccl_2_6/260-6312002-1868754?pd_rd_w=rXjBd&content-id=amzn1.sym.716ad392-9d85-4575-b34c-e8d1201b1a5a&pf_rd_p=716ad392-9d85-4575-b34c-e8d1201b1a5a&pf_rd_r=HCPQRKY9MH3J0Y3J6GA1&pd_rd_wg=ltg3y&pd_rd_r=191a716d-4f1b-4400-8250-c0b74acb6f6b&pd_rd_i=B07P84JGLB&psc=1)
6. **Connecting Wires**
7. **Breadboard (for prototyping)**
8. **5V DC Power Supply (for relay and sensor)**

### **Component Descriptions:**

- **Moisture Sensor (Soil Hygrometer Humidity Detection Module with LM393 Comparator Chip):**

  - **Operating Voltage:** 3.3V~5V.
  - **Output Type:** Digital (DO) and Analog (AO).
  - **Digital Output (DO):** High when the soil is dry, Low when the soil is moist.

- **Relay Module (5V One Channel):**

  - Allows control of high-power devices (like motors) with a low-power signal from the microcontroller.
  - Operates on a 5V signal from the ESP32.

- **Buzzer (Small Enclosed Piezo Electronic Buzzer):**
  - **Operating Voltage:** 3.5-12V.
  - Emits a loud sound (95dB) when powered.

### **Circuit Connections:**

#### **1. Moisture Sensor to ESP32:**

- **VCC** (Sensor) → **3.3V** (ESP32)
- **GND** (Sensor) → **GND** (ESP32)
- **DO** (Digital Output, Sensor) → **GPIO32** (ESP32)

#### **2. Relay Module to ESP32 and LED (Simulated Motor):**

- **VCC** (Relay) → **5V** (External Power Supply)
- **GND** (Relay) → **GND** (ESP32)
- **IN** (Relay Control Input) → **GPIO23** (ESP32)
- **COM** (Relay) → **5V** (External Power Supply)
- **NO (Normally Open)** (Relay) → **Anode** (Longer leg of the LED)
- **Cathode** (Shorter leg of the LED) → **GND** (ESP32)

#### **3. Buzzer to ESP32:**

- **Positive Terminal (Buzzer)** → **GPIO22** (ESP32)
- **Negative Terminal (Buzzer)** → **GND** (ESP32)

### **Important Notes:**

- **Use a common ground for all components connected to the ESP32 and external power supplies.**
- **Ensure that the external power supply is correctly rated (5V DC) for the relay and moisture sensor.**

### **MicroPython Code:**

```python
from machine import Pin
from time import sleep

# Moisture sensor connected to digital pin
moisture_sensor = Pin(32, Pin.IN)

# Relay control pin
relay_pin = Pin(23, Pin.OUT)

# Buzzer pin
buzzer_pin = Pin(22, Pin.OUT)

def check_soil_moisture():
    """Check soil moisture level and control motor and buzzer."""
    if moisture_sensor.value() == 1:  # Soil is dry
        relay_pin.on()  # Turn on the relay (motor/LED)
        buzzer_pin.on()  # Turn on the buzzer
        print("Soil is dry. Motor (LED) ON and Buzzer ON")
    else:  # Soil is moist
        relay_pin.off()  # Turn off the relay (motor/LED)
        buzzer_pin.off()  # Turn off the buzzer
        print("Soil is moist. Motor (LED) OFF and Buzzer OFF")

while True:
    check_soil_moisture()
    sleep(5)  # Check every 5 seconds
```

### **Code Explanation:**

1. **Moisture Sensor Reading:**
   - The sensor provides a digital output signal: High (1) when soil is dry, Low (0) when soil is moist.
2. **Relay and Buzzer Control:**

   - When the soil is dry, the relay (and thus the simulated motor with LED) and the buzzer are turned on.
   - When the soil is moist, the relay and buzzer are turned off.

3. **Looping Mechanism:**
   - The system checks the moisture level every 5 seconds.

### **Assembly Guide:**

1. **Mount the ESP32 and Components:**

   - Use a breadboard to connect all components for easy assembly and testing.

2. **Power Connections:**

   - Ensure the moisture sensor and relay module are powered with 3.3V and 5V, respectively.

3. **MicroPython Environment Setup:**

   - Install the necessary MicroPython environment and upload the code to the ESP32.

4. **Testing:**
   - Upload the code and open the Serial Monitor to observe the output.
   - Test the system by placing the moisture sensor in dry and wet soil to see the response.

### **Safety Precautions:**

- **Ensure proper voltage levels:** Double-check the connections and power supply to avoid over-voltage that could fry the ESP32.
- **Relay Handling:** When dealing with relays, always make sure you understand the current and voltage requirements of the devices you're controlling.
- **Proper Grounding:** Ensure all components share a common ground to prevent circuit malfunctions.
