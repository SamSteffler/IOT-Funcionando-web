<script setup>
import { ref, computed } from 'vue'

const selectedPin = ref(null)
const pins = ref([
  { number: 14, capabilities: ['touch', 'ADC', 'PWM'], usable: true, value: 0, notes: '' },
  { number: 13, capabilities: ['touch', 'ADC', 'PWM'], usable: true, value: 0, notes: '' },
  { number: 12, capabilities: ['touch', 'ADC', 'PWM'], usable: true, value: 0, notes: '' },
  { number: 10, capabilities: ['touch', 'ADC', 'PWM'], usable: true, value: 0, notes: '' },
  { number: 9, capabilities: ['touch', 'ADC', 'PWM'], usable: true, value: 0, notes: '' },
  { number: 46, capabilities: ['PWM'], usable: true, value: 0, notes: 'log' },
  { number: 3, capabilities: ['touch', 'ADC', 'PWM'], usable: true, value: 0, notes: '' },
  { number: 8, capabilities: ['touch', 'ADC', 'PWM'], usable: true, value: 0, notes: '' },
  { number: 18, capabilities: ['ADC', 'PWM'], usable: true, value: 0, notes: '' },
  { number: 17, capabilities: ['ADC', 'PWM'], usable: true, value: 0, notes: '' },
  { number: 16, capabilities: ['ADC', 'PWM'], usable: true, value: 0, notes: '' },
  { number: 15, capabilities: ['ADC', 'PWM'], usable: true, value: 0, notes: '' },
  { number: 7, capabilities: ['touch', 'ADC', 'PWM'], usable: true, value: 0, notes: '' },
  { number: 6, capabilities: ['touch', 'ADC', 'PWM'], usable: true, value: 0, notes: '' },
  { number: 5, capabilities: ['touch', 'ADC', 'PWM'], usable: true, value: 0, notes: '' },
  { number: 4, capabilities: ['touch', 'ADC', 'PWM'], usable: true, value: 0, notes: '' },
  { number: 19, capabilities: ['ADC', 'PWM'], usable: true, value: 0, notes: 'USB D+ (OTG)' },
  { number: 20, capabilities: ['ADC', 'PWM'], usable: true, value: 0, notes: 'USB D- (OTG)' },
  { number: 21, capabilities: ['PWM'], usable: true, value: 0, notes: '' },
  { number: 47, capabilities: ['PWM'], usable: true, value: 0, notes: '' },
  { number: 48, capabilities: ['PWM'], usable: true, value: 0, notes: 'WS2812 built-in' },
  { number: 45, capabilities: ['PWM'], usable: false, value: 0, notes: 'VSPI - Internal' },
  { number: 0, capabilities: ['PWM'], usable: false, value: 0, notes: 'Boot - Internal' },
  { number: 35, capabilities: ['PWM'], usable: false, value: 0, notes: 'PSRAM - Internal' },
  { number: 36, capabilities: ['PWM'], usable: false, value: 0, notes: 'PSRAM - Internal' },
  { number: 37, capabilities: ['PWM'], usable: false, value: 0, notes: 'PSRAM - Internal' },
  { number: 38, capabilities: ['PWM'], usable: true, value: 0, notes: 'SD_CMD (if SD is used)' },
  { number: 39, capabilities: ['PWM'], usable: true, value: 0, notes: 'SD_CLK (if SD is used)' },
  { number: 40, capabilities: ['PWM'], usable: true, value: 0, notes: 'SD_DATA (if SD is used)' },
  { number: 41, capabilities: ['PWM'], usable: true, value: 0, notes: '' },
  { number: 42, capabilities: ['PWM'], usable: true, value: 0, notes: '' },
  { number: 2, capabilities: ['touch', 'ADC', 'PWM'], usable: true, value: 0, notes: 'LED built-in' },
  { number: 1, capabilities: ['touch', 'ADC', 'PWM'], usable: true, value: 0, notes: '' },
  { number: 'RX', capabilities: ['PWM'], usable: true, value: 0, notes: 'UART RX' },
  { number: 'TX', capabilities: ['PWM'], usable: true, value: 0, notes: 'UART TX' },
])

const usablePins = computed(() => pins.value.filter(pin => pin.usable))
const unusablePins = computed(() => pins.value.filter(pin => !pin.usable))

const selectPin = (pin) => {
  selectedPin.value = pin
}

const configurePin = () => {
  if (selectedPin.value) {
    console.log('Configuring pin:', selectedPin.value.number)
    // Add your configuration logic here
  }
}

const readValue = () => {
  if (selectedPin.value) {
    console.log('Reading value from:', selectedPin.value.number)
    // Add your read logic here
  }
}
</script>

<template>
  <div class="esp32-interface">
    <div class="board-section">
      <h2>ESP32-S3 Board</h2>
      <div class="board-container">
        <img src="/ESP32S3_Pinout.png" alt="ESP32-S3 Board" class="board-image" />
        <div class="pin-buttons">
          <div class="pin-group">
            <h4>Usable Pins</h4>
            <button 
              v-for="pin in usablePins" 
              :key="pin.number"
              @click="selectPin(pin)"
              :class="['pin-button', { active: selectedPin?.number === pin.number }]"
            >
              GPIO {{ pin.number }}
            </button>
          </div>
          <div class="pin-group">
            <h4>Internal Pins</h4>
            <button 
              v-for="pin in unusablePins" 
              :key="pin.number"
              @click="selectPin(pin)"
              :class="['pin-button', 'unusable', { active: selectedPin?.number === pin.number }]"
            >
              GPIO {{ pin.number }}
            </button>
          </div>
        </div>
      </div>
    </div>

    <div class="config-section">
      <h2>Configuration & Readings</h2>
      <div v-if="selectedPin" class="config-panel">
        <div class="pin-header">
          <h3>GPIO {{ selectedPin.number }}</h3>
          <span v-if="!selectedPin.usable" class="badge-unusable">⚠️ Internal Pin</span>
        </div>
        
        <div class="config-item">
          <label>Capabilities:</label>
          <div class="capabilities">
            <span v-for="cap in selectedPin.capabilities" :key="cap" class="capability-badge">
              {{ cap }}
            </span>
          </div>
        </div>

        <div v-if="selectedPin.notes" class="config-item">
          <label>Notes:</label>
          <span class="notes">{{ selectedPin.notes }}</span>
        </div>

        <div class="config-item">
          <label>Current Value:</label>
          <span class="value">{{ selectedPin.value }}</span>
        </div>

        <div class="config-item">
          <label>Status:</label>
          <span :class="['status', selectedPin.usable ? 'usable' : 'unusable']">
            {{ selectedPin.usable ? '✓ Available' : '✗ Not Available' }}
          </span>
        </div>

        <div v-if="selectedPin.usable" class="config-actions">
          <button class="btn-primary" @click="configurePin">Configure</button>
          <button class="btn-secondary" @click="readValue">Read Value</button>
        </div>
        <div v-else class="warning-message">
          ⚠️ This pin is reserved for internal use and cannot be configured
        </div>
      </div>
      <div v-else class="placeholder">
        <p>👈 Select a pin to view configuration and readings</p>
      </div>
    </div>
  </div>
</template>

<style scoped>
.esp32-interface {
  display: flex;
  height: 100vh;
  gap: 20px;
  padding: 20px;
  box-sizing: border-box;
  background: #f0f2f5;
}

.board-section {
  flex: 1;
  display: flex;
  flex-direction: column;
}

.board-container {
  position: relative;
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  background: #ffffff;
  border-radius: 12px;
  padding: 20px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

.board-image {
  max-width: 100%;
  max-height: 100%;
  object-fit: contain;
}

.pin-buttons {
  position: absolute;
  top: 20px;
  right: 20px;
  display: flex;
  flex-direction: column;
  gap: 16px;
  max-height: calc(100% - 40px);
  overflow-y: auto;
  padding-right: 8px;
}

.pin-buttons::-webkit-scrollbar {
  width: 6px;
}

.pin-buttons::-webkit-scrollbar-track {
  background: #f1f1f1;
  border-radius: 3px;
}

.pin-buttons::-webkit-scrollbar-thumb {
  background: #cbd5e0;
  border-radius: 3px;
}

.pin-buttons::-webkit-scrollbar-thumb:hover {
  background: #a0aec0;
}

.pin-group {
  display: flex;
  flex-direction: column;
  gap: 6px;
}

.pin-group h4 {
  font-size: 12px;
  color: #4a5568;
  font-weight: 600;
  margin: 0 0 4px 0;
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

.pin-button {
  padding: 6px 12px;
  background: #4a5568;
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  font-size: 13px;
  font-weight: 500;
  transition: all 0.3s;
  min-width: 100px;
  text-align: left;
}

.pin-button:hover {
  background: #2d3748;
  transform: translateX(-4px);
}

.pin-button.active {
  background: #3182ce;
  box-shadow: 0 2px 8px rgba(49, 130, 206, 0.4);
}

.pin-button.unusable {
  background: #e53e3e;
  opacity: 0.7;
}

.pin-button.unusable:hover {
  background: #c53030;
}

.config-section {
  flex: 1;
  display: flex;
  flex-direction: column;
  background: #ffffff;
  border-radius: 12px;
  padding: 20px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

.config-panel {
  flex: 1;
}

.config-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 16px;
  margin: 12px 0;
  background: #f7fafc;
  border-radius: 8px;
  border-left: 4px solid #3182ce;
}

.config-item label {
  font-weight: 600;
  color: #2d3748;
  font-size: 14px;
}

.config-item span {
  color: #4a5568;
  font-size: 14px;
}

.config-item .value {
  font-weight: 600;
  color: #3182ce;
  font-size: 16px;
}

.config-actions {
  margin-top: 24px;
  display: flex;
  gap: 12px;
}

.btn-primary, .btn-secondary {
  flex: 1;
  padding: 12px 24px;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-size: 14px;
  font-weight: 600;
  transition: all 0.3s;
}

.btn-primary {
  background: #3182ce;
  color: white;
}

.btn-primary:hover {
  background: #2c5282;
  box-shadow: 0 4px 12px rgba(49, 130, 206, 0.3);
}

.btn-secondary {
  background: #e2e8f0;
  color: #2d3748;
}

.btn-secondary:hover {
  background: #cbd5e0;
}

.placeholder {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  color: #718096;
  font-size: 16px;
}

h2 {
  margin: 0 0 20px 0;
  color: #2d3748;
  font-size: 24px;
  font-weight: 700;
}

h3 {
  margin: 0;
  color: #2d3748;
  font-size: 20px;
  font-weight: 600;
}

.pin-header {
  display: flex;
  align-items: center;
  gap: 12px;
  margin-bottom: 20px;
  padding-bottom: 12px;
  border-bottom: 2px solid #e2e8f0;
}

.badge-unusable {
  padding: 4px 12px;
  background: #fed7d7;
  color: #c53030;
  border-radius: 4px;
  font-size: 12px;
  font-weight: 600;
}

.capabilities {
  display: flex;
  gap: 8px;
  flex-wrap: wrap;
}

.capability-badge {
  padding: 4px 10px;
  background: #bee3f8;
  color: #2c5282;
  border-radius: 4px;
  font-size: 12px;
  font-weight: 600;
}

.notes {
  color: #718096;
  font-style: italic;
}

.status {
  font-weight: 600;
}

.status.usable {
  color: #38a169;
}

.status.unusable {
  color: #e53e3e;
}

.warning-message {
  margin-top: 24px;
  padding: 16px;
  background: #fff5f5;
  border: 1px solid #feb2b2;
  border-radius: 8px;
  color: #c53030;
  font-size: 14px;
  font-weight: 500;
  text-align: center;
}

/* Responsive design */
@media (max-width: 768px) {
  .esp32-interface {
    flex-direction: column;
    height: auto;
  }
  
  .pin-buttons {
    position: static;
    flex-direction: row;
    flex-wrap: wrap;
    margin-top: 16px;
    max-height: none;
  }
  
  .pin-group {
    flex: 1;
    min-width: 200px;
  }
}
</style>
