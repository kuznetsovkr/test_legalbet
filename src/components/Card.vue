<template>
    <div style="display: flex; flex-direction: column; justify-content: center; align-items: center; height: 100vh;">   
        <div class="forecast-card">
            <div class="container">
                <div class="forecast-card__header">
                    <img src="/src/assets/images/face.png" alt="Author" class="forecast-card__header--avatar" />
                    <div class="forecast-card__header--info">
                      <p class="forecast-card__header--info-name">{{ forecastData.author }}</p>

                      <p class="forecast-card__header--info-stats">
                        Статистика за последние 30 дней
                      </p>

                      <div class="forecast-card__header--info-mobile-stats">
                        <span class="plus">{{ forecastData.plus }}</span> /
                        <span class="equally">{{ forecastData.equally }}</span> /
                        <span class="minus">{{ forecastData.minus }}</span> /
                        <span class="roi">ROI {{ forecastData.roi }}%</span>
                      </div>
                    </div>

                    <div class="forecast-card__header--mobile-summary">
                        <p class="forecast-card__header--mobile-summary-days">30 дней</p>
                        <p
                        class="forecast-card__header--mobile-summary-profit"
                        :class="{ plus: forecastData.profit > 0, minus: forecastData.profit < 0 }"
                        >
                        {{ forecastData.profit > 0 ? '+' : '' }}
                        {{ Math.abs(forecastData.profit).toLocaleString('ru-RU') }}
                        </p>
                    </div>


        
                    <div class="forecast-card__header--stat">
                        <div class="forecast-card__header--stat-plus">
                            <p>+</p>
                            <div class="divider"></div>
                            <p>{{ forecastData.plus }}</p>
                        </div>
                        <div class="forecast-card__header--stat-equally">
                            <p>=</p>
                            <div class="divider"></div>
                            <p>{{ forecastData.equally }}</p>
                        </div>
                        <div class="forecast-card__header--stat-minus">
                            <p>—</p>
                            <div class="divider"></div>
                            <p>{{ forecastData.minus }}</p>
                        </div>
                        <div class="forecast-card__header--stat-roi">
                            <p class="forecast-card__header--stat-roi-firstRow">ROI, %</p>
                            <div class="divider"></div>
                            <p>{{ forecastData.roi }} %</p>
                        </div>
                        <div class="forecast-card__header--stat-profit">
                            <p class="forecast-card__header--stat-profit-firstRow">Прибыль</p>
                            <div class="divider"></div>
                            <p>
                                {{ forecastData.profit > 0 ? '+' : '' }}
                                {{ Math.abs(forecastData.profit).toLocaleString('ru-RU') }}
                            </p>
                        </div>
                    </div>
                </div>
    
                <div class="forecast-card__content" :style="{ maxHeight: maxHeight, overflow: 'hidden' }" ref="contentRef">
                    <p>{{ forecastData.text }}</p>
                    <div v-if="!isExpanded" class="content-fade"></div>
                </div>
                <div class="toggle-button-wrapper">
                    <button class="toggle-button" @click="toggleContent">
                    <img
                        src="/src/assets/images/arrow-down.svg"
                        alt="toggle"
                        :class="{ rotated: isExpanded }"
                    />
                    </button>
                </div>
            </div>
    
            <div class="forecast-card__footer">
                <div class="forecast-card__footer-inner container">
                    <div class="forecast-card__footer--bet">
                        <p class="forecast-card__footer--bet-type">ПРОГНОЗ</p>
                        <p class="forecast-card__footer--bet-name">{{ forecastData.betName }}</p>
                    </div>
                    <div class="forecast-card__footer--bk">
                        <img src="/src/assets/images/1Хstavka.svg" alt="БК" class="bk-icon" />
                        <div class="forecast-card__footer--bk-text">
                        <p class="forecast-card__footer--bk-text-kf">Коэффициент</p>
                        <p class="forecast-card__footer--bk-text-number">  {{ forecastData.coefficient.toFixed(2) }}</p>
                        </div>
                    </div>
                    <button class="forecast-card__footer--btn">
                        <p>Сделать ставку</p>
                    </button>
                </div>
            </div>
        </div>
    </div>
  </template>
  
<script setup lang="ts">
  import { ref, onMounted, nextTick } from 'vue';
  
  const isExpanded = ref(false);
  const maxHeight = ref('7.5em');
  const contentRef = ref<HTMLElement | null>(null);
  
  // Мокнутые данные
  const forecastData = ref({
    author: 'Владимир Вуйтек',
    plus: 647,
    equally: 29,
    minus: 413,
    roi: 23.9,
    profit: 22820,
    text: `В Кубке страны команда Унаи Эмери бессовестно размялась на «Виктории» из Ла-Коруньи (8:0). 
    При этом серьёзные соперники: «Севилья», «Барселона» (даже кризисная) и «МЮ» (ещё более кризисный) — били «Вильярреал». 
    ЛЧ не турнир для Эмери. Он гроссмейстер убогой ЛЕ.
    Поэтому у «Аталанты» есть шансы. Ей необходимо играть лишь на победу. 
    Только выигрыш выводит её в плей-офф ЛЧ. А поражение в комплекте с победой «Янг Бойз» над «МЮ» оставляет бергамасков 
    без еврокубковой весны.
    Гасперини вполне по силам одолеть команду Эмери. «Аталанта» уже обыгрывала недавно «Юве» и «Наполи». 
    Поставлю на победу итальянского клуба.`,
    betName: 'Штрафное время: тотал больше 16.5',
    coefficient: 3.20
  });
  
  const toggleContent = async () => {
    isExpanded.value = !isExpanded.value;
    await nextTick();
    if (contentRef.value) {
      maxHeight.value = isExpanded.value
        ? contentRef.value.scrollHeight + 'px'
        : '7.5em';
    }
  };
  
  onMounted(() => {
    if (contentRef.value) {
      maxHeight.value = '7.5em';
    }
  });
</script>

<style scoped lang="scss">
  .forecast-card__content {
    position: relative;
    transition: max-height 0.5s ease;
    p {
      line-height: 1.5em;
      font-size: 16px;
      color: rgba(0, 0, 0, 0.6);
    }

    .content-fade {
      position: absolute;
      bottom: 0;
      left: 0;
      right: 0;
      height: 3em;
      background: linear-gradient(180deg, rgba(255, 255, 255, 0) 0%, #fff 100%);
      pointer-events: none;
    }
  }


  .forecast-card__footer {
    background: rgba(240, 241, 242, 0.4);
    border-left: 2px solid #3498db;
    padding: 24px 0;
    position: relative;
    z-index: 1;
  }
  .toggle-button-wrapper {
    position: relative;
    height: 0;
    z-index: 3;
  }

  .toggle-button {
    position: absolute;
    left: 50%;
    transform: translateX(-50%);
    background: #fff;
    border: none;
    border-radius: 20px;
    width: 40px;
    height: 40px;
    display: flex;
    justify-content: center;
    align-items: center;
    cursor: pointer;
    z-index: 3;
    box-shadow: 2px 4px 12px 0 rgba(0, 0, 0, 0.08);
    transition: background 0.3s ease;

    img {
      width: 16px;
      height: 16px;
      transition: transform 0.3s ease;
    }

    img.rotated {
      transform: rotate(180deg);
    }
  }

</style>