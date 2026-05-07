#include <SchooMyUtilities.h>
SchooMyUtilities scmUtils = SchooMyUtilities();

// 明るさセンサー: 左上ポート
// スピーカー: 左下ポート
// 音センサー: 右上ポート

#define BRIGHTNESS_PIN A5   // 左上アナログ
#define SPEAKER_PIN 5       // 左下 secondピン
#define SOUND_PIN A1        // 右上アナログ

#define BRIGHT_THRESHOLD 600   // この値より明るくなったらアラーム開始
#define SOUND_THRESHOLD  400   // この値より大きい音で叫んだと判定

bool alarmActive = false;

void setup() {
  Serial.begin(9600);
  pinMode(SPEAKER_PIN, OUTPUT);
  pinMode(SOUND_PIN, INPUT);
}

void loop() {
  int brightness = _sbeGetBrightness(BRIGHTNESS_PIN);
  int soundLevel = analogRead(SOUND_PIN);

  Serial.print("明るさ: ");
  Serial.print(brightness);
  Serial.print("  音: ");
  Serial.println(soundLevel);

  // 明るくなったらアラーム開始
  if (brightness > BRIGHT_THRESHOLD) {
    alarmActive = true;
  }

  // アラームが鳴っているとき
  if (alarmActive) {
    tone(SPEAKER_PIN, 1000); // 1000Hzの音を鳴らす

    // 叫び声を検知したら止める
    if (soundLevel > SOUND_THRESHOLD) {
      alarmActive = false;
      noTone(SPEAKER_PIN);
      Serial.println("アラーム停止！");
      delay(2000); // 誤検知防止のため少し待つ
    }
  } else {
    noTone(SPEAKER_PIN);
  }

  delay(100);
}

float _sbeGetBrightness(int pin) {
  int raw = analogRead(pin);
  return (float)(1023 - raw) / 1023.0 * 100.0;
}
