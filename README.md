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
        println("제가 바라보는 제 친구입니다.")
        println("스스로 작은 개발자라고 하지만 자기 위치를 알고서도 스스로를 존대하는 친구입니다.")
        println("가끔 큰 실수를 하기도 하지만, 그런 점이 약점이 되지 않길 원합니다.\n")
        var langs = ""
        langArray.forEach {
            langs += "$it,"
        }
        println("할 수 있는 언어: $langs")
        var prLangs = ""
        programmingLangArray.forEach {
            prLangs += "$it,"
        }
        println("할 수 있는 프로그래밍 언어: $prLangs")
        println("정보: ")
        println("	이름: 배현우")
        println("	나이: 17(15)")
        println("	성: 남")
        println("	특이사항: 저와 같이 귀찮아 하는 친구입니다. 그래도 저처럼 인생에 길을 잃은 친구는 아닙니다.")
        println("연락 방법: ")
        println("Twitter: 안해요. 제가 트위터 자체가 없어요.")
        println("Discord: 자주 연락이 끊겨요. 가끔은 제가 오해할 정도로요.")
    }
    else if (properties.readText(StandardCharsets.UTF_8) == "en") {
        println("Let me introduce my friend.")
        println("He said he is a small developer himself, but he knows himself.")
        println("Sometimes makes a big mistake, I wish that could not be disadvantage for him.\n")
        var langs = ""
        langArray.forEach {
            langs += "$it,"
        }
        println("What can i speak: $langs")
        var prLangs = ""
        programmingLangArray.forEach {
            prLangs += "$it,"
        }
        println("What i use for Programming: $prLangs")
        println("Identities: ")
        println("	Name: Bae Hyeon Woo")
        println("	Age: 17(15)")
        println("	Gender: Male")
        println("	Uniqueness: Tired everything like me. but he does not lose way for life like me.")
        println("How to Contact: ")
        println("Twitter: Don't. i don't have")
        println("Discord: Sometimes disconnects. even i can make wrong thinks.")
    }
}
```
