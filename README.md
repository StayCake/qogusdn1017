```kt
package com.baehyeonwoo.hyeonidentities

import java.io.File
import java.nio.charset.StandardCharsets

private val langArray = arrayListOf (
    "Korean",
    "English"
)

private val programmingLangArray = arrayListOf (
    "Kotlin"
)

private val properties = File("/root/.config/HyeonIdentities/config.txt")

fun main() {
    if (properties.readText(StandardCharsets.UTF_8) == "ko") {
        println("안녕하세요. 평범한 개발자 배현우입니다.")
        println("개발자인 것 같지만 실상 넓은 분야로 많이 개발하지는 않는 작은 개발자입니다.")
        println("많이 정신이 나간 놈이라 친해지려면 여러번 생각해 보시는게 좋을 것 같습니다.")
        println("할 수 있는 언어: ${langArray.toString().replace("[", "").replace("]", "")}")
        println("할 수 있는 프로그래밍 언어: ${programmingLangArray.toString().replace("[", "").replace("]", "")}\n")
        println("정보: \n")
        println("	이름: 배현우\n")
        println("	나이: 17(15)\n")
        println("	성: 남\n")
        println("	특이사항: 미친 새끼입나다. 모든것을 힘들어하고 귀찮아 합니다.\n")
        println("연락처: \n")
        println("Twitter (주 계정 / 한국 마인크래프트 유튜버 관련): @qogusdn1017\n")
        println("Twitter (주 계정 / 개발 관련): @baehyeonwoo1017\n")
        println("Discord: 비공개입니다. 원하신다면 저와 굉장히 친해지셔야합니다.\n")
    }
    else if (properties.readText(StandardCharsets.UTF_8) == "en") {
        println("Hi, this is just normal developer named Bae Hyeon Woo.")
        println("Seems to be a developer, but in reality it's just a small developer who does not develop much in a wide field.")
        println("I'm very crazy person, so I think it would be better to think several times to get closer with me.\n\n")
        println("Langauge which I can do: ${langArray.toString().replace("[", "").replace("]", "")}")
        println("Programming languages which I can do: ${programmingLangArray.toString().replace("[", "").replace("]", "")}\n")
        println("Identities: \n")
        println("	Name: Bae Hyeon Woo\n")
        println("	Age: 17(15)\n")
        println("	Gender: Male\n")
        println("	Uniqueness: Crazy bitch. Tired + bored of everything.\n")
        println("Contact: \n")
        println("Twitter (Main / Korean Minecraft YouTubers Related): @qogusdn1017\n")
        println("Twitter (Main / Dev Related): @baehyeonwoo1017\n")
        println("Discord: Private, you need to be best friends with me if you want it.\n")
    }
}
```
