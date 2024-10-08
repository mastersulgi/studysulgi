# STUDYSULGI
이 글에서는 VariableTrigger V1 에 대한 내용을 다룹니다.
이는 신규 개발자들이 VariableTrigger 를 이용하여 새로운 기능을 구현하는 한편,
기존 개발자가 개발하는데에 있어서 참고하기 위한 Reference 의 역할을 합니다.

# 저작권의 표시
이 글의 기본 저작권은 Bukkit 에게 있으며, 설명 및 사용 예시는 @mastersulgi 에게 있습니다만, 편하게 레퍼런스 하셔도 무방합니다.
여기에 적히는 내용들은 시간이 있을 때 최대한 꼼꼼하게 작성할 예정이며, 장시간 업데이트되지 않을 수도 있습니다.

# 시작
https://dev.bukkit.org/projects/variabletriggers

- 공식 홈페이지에서 제공하고 있는 VariableTriggers 에 대한 정보.
- 현재 VTV1, VTV2 두 개의 버전이 있지만, VTV2 는 만들어놓거나 작성해놓은 스크립트 및 파일이 날아가는 오류가 있어서 본 글은 VTV1 을 기준으로 설명합니다.
- 이미 VariableTriggers 를 다루고 있는 수십개의 글이 있지만, 대부분 활용 예제 없이 Bukkit 에서 제공하는 원문을 번역한 것에서 그치는 것으로 보여 해당 글을 작성합니다.

# 플러그인에 대한 설명
VariableTriggers 는 스크립트 계열 플러그인 중에서 가장 기능을 구현하기 쉬운 대신 확장성이 떨어진다는 단점을 가지고 있습니다. 한국에서 사용되는 Skript 플러그인 및 TriggerReactor 와 같은 플러그인은 경우에 따라서 내부에 변수를 저장해두었다가 필요한 경우 꺼내오는 것이 가능한데, VariableTriggers 는 그러한 방식을 쓰는 것이 대단히 제한적입니다.

따라서 VariableTriggers 를 사용하여 무언가를 구현할때는 굉장히 원초적인 접근을 할 필요가 있습니다. '이런 부분까지 코딩을 해줘야 하나?' 라는 의구심이 든다면, 일단 코딩을 해주고 나서 나중에 수정하는 것이 문제가 적습니다.

현재는 VariableTriggers V1, V2 모두 새로운 기능이 업데이트되고 있지 않으니 추후 플러그인의 오류가 수정될 가능성은 사실 상 없다시피하나, VariableTriggers V1 의 경우 출시된 이례 지금까지 쭉 완성형 플러그인이었기 때문에 기본적인 문법(syntax) 를 익히고 이를 활용하여 새로운 기능을 구현하기가 대단히 용이합니다.

# 개발을 시작하기에 앞서: 마인드셋
이 단락은 VariableTriggers 플러그인을 사용하는데에 있어서의 마인드셋이라기 보다는, '코딩을 통해 내가 원하는 기능을 구현하는 것' 에 초점을 맞춘다고 보시면 됩니다. 일반적으로 처음 코딩을 시작하는 분들이 인터넷 블로그나 웹사이트, 책에 나와있는 내용을 그대로 따라 적은 뒤 이를 실행하고, 나오는 결과값을 외운 뒤 '이런 결과값이 나오게 코딩할 줄 안다' 라고 착각하시는 경우가 있습니다.

이는 아주 간단한 기능을 구현하는데에 있어서는 효율적일지 모르겠으나, 복잡하고 다양한 기능을 모두 이용해야 하는 경우에는 앞서 책이나 블로그에 나와있는 코드를 그대로 적고 원하는 결과값이 나오지 않아 당황스러울 수 있습니다. 따라서 필자는 코딩을 하는데에 있어서 다음과 같은 마인드셋을 가질 것을 제안합니다:

- 내가 원하는 기능이 작동하는 원리를 먼저 생각해보자.
- 내가 만들고자 하는 기능이 이미 어디선가 제공되고 있는지 알아보자.

내가 구현하고자 하는 기능이 작동하는 원리를 그림으로 그려도 좋고, 글로 써도 좋습니다. 일반적으로 이러한 과정을 '모식도를 그린다' 고 표현합니다만, 중요한 건 '스스로가 만들고 있는 걸 어떤 원리로서 돌아가게 할 것인가?' 를 끊임없이 생각하는 것입니다.

내가 만들고자 하는 기능이 이미 어디선가 제공되고 있는 내용이라면, 해당 내용을 레퍼런스 삼아도 크게 상관이 없습니다. 단지, 처음에 언급한 '원리의 이해' 가 반드시 이행되어야 합니다. 원리를 이해하지 않고 코드를 복사해서 붙여넣기만 한다면, 똑같은 문제에 직면했을때라면 몰라도 이를 응용한 비슷하지만 다른 상황에 무기력해질 수 있습니다.

# VariableTriggers 의 기능
VariableTriggers 에는 Area / Click / Command / Event / Inventory / Walk 의 총 6가지 이벤트에 대한 트리거를 작성할 수 있습니다. 각각의 이벤트에 따라 사용할 수 있는 기능도 다르며, 작동하는 방식도 다릅니다.

그리고 6가지 종류의 트리거를 모두 다룰 수 있는 Script 트리거를 작성할 수 있습니다. 본 글에서는 이 Script 를 이용하거나 다루지는 않을 예정이지만, 언젠가 여건이 된다면 이 부분에 대해서도 충분히 설명을 추가하도록 하겠습니다.

트리거는 크게 다음으로 나눌 수 있습니다.
- 연산자 (@PLAYER, @CMD, @IF)
- 플레이스 홀더 (\<playername>,\<health>)
- 동적 플레이스 홀더 (\<cmdarg:1>,\<haspermission.*>)
- EventTriggers (PlayerDeath, Join)
- 명령문 (gamemode 1, gamemode 0)

괄호 안에 적혀 있는 건 그저 예시일 뿐, 훨씬 더 많은 연산자와 플레이스 홀더들이 기다리고 있습니다. 이러한 내용들에 대해서 아주 자세히 설명해보도록 하겠습니다.

# 연산자 (@)

연산자는 VariableTriggers 의 기능 구현의 시작과 끝을 담당하고 있습니다. 일반적으로 어떤 명령을 실행하기 위한 호출부호로 쓰이므로 호출 부호로 불리는 경우도 있습니다만, 본 글에서는 연산자로 통칭하도록 하겠습니다.

연산자는 명령문보다 앞에 쓰이며, 명령이 실행되는 옵션을 지정해준다고 생각하면 편합니다. 이러한 연산자를 이용한 옵션이 지정되지 않은 명령은 VariableTriggers 내에서 받아들여지지 않으므로, 빼먹었을 때 자신이 원하는대로 기능이 작동하지 않는 것은 당연지사 일겁니다.

수 많은 연산자가 존재하지만 중요도에 따라서 Bukkit Wiki 에 등재되어 있는 순서와는 다르게 글이 작성되어 있으니, 찾고 싶은 연산자를 검색하여 사용법을 확인하시는 것도 하나의 방법이 될 수 있겠습니다.

# 권한에 대한 연산자

- @CMD - 이후에 오는 명령을 명령을 실행하는 플레이어의 권한으로 실행합니다.

가령 op 권한이 없는 플레이어가 게임모드를 크리에이티브로 변경하려고 하면 변경하지 못하듯이, 이 연산자는 명령을 실행하는 플레이어가 온전히 자신만의 권한으로 명령을 실행하게끔 해줍니다. 그럼 의문점이 생길 수도 있습니다. '내 권한이 아닌, 다른 권한으로 실행한다는게 뭔소리인가?'

- @CMDOP - 이후에 오는 명령을 명령을 실행하는 플레이어가 op 권한을 가지고 있는 것으로 간주하고 실행합니다.
- @CMDCON - 이후에 오는 명령을 명령을 실행하는 플레이어가 콘솔의 권한을 가지고 있는 것으로 간주하고 실행합니다.

위에서 언급된 @CMD 보다도, 이 2개의 연산자가 가장 많이, 그리고 자주 쓰입니다. 일반적으로는 실행할 수 없는 명령이지만, 해당 연산자를 사용함으로써 명령을 실행하는 플레이어가 더 상위의 권한을 가지고 실행할 수 있게끔 해줍니다.

만약 내가 op 권한이 없는데, 게임모드를 쓸 수 있게끔 만들고 싶다면 어떤식으로 구현하는 편이 좋을까요?

@CMDOP gamemode 1 - 실행하는 사람이 op 권한이 있는 것으로 간주하고 gamemode 1 명령을 실행 

이렇게 간단하게 자신의 권한을 상승하여 뒤에 오는 명령을 실행할 수 있게끔 해주는 것입니다.

# 메시지에 관련된 연산자

- @PLAYER - 이후에 적힌 문장을 명령을 실행하는 플레이어에게 출력합니다.
- @BROADCAST - 이후에 적힌 문장을 모든 플레이어가 볼 수 있게 공지합니다.
- @QUIET [playername] [seconds] - [playername] 의 채팅을 [seconds] 동안 보이지 않게 합니다.
- @CLEARCHAT [playername] - [playername] 의 채팅창을 초기화합니다.
- @TELL [playername] - 이후에 적힌 문장을 [playername] 에게 출력합니다.
- @PRINT - 이후에 적힌 문장을 콘솔에 표시합니다.
- @! - 디버그를 위한 정보를 명령을 실행하는 플레이어에게 출력합니다.

이 연산자들은 실행한 플레이어 혹은 특정한 플레이어에게 지정해놓은 메시지를 출력하게끔 해줍니다. 사실 그 외에 설명할 내용이 없어서 간략하게 넘어갑니다.

# 제어 연산자

- @PAUSE [seconds] - [seconds] 만큼 트리거의 진행을 멈춥니다.

위키에 등재되어 있는 문서에는 @COOLDOWN [seconds] 가 나와있긴 합니다만, 제대로 작동되지도 않을 뿐더러 오류를 수없이 뿜어내기에 보통 @PAUSE 구문을 많이 사용합니다. 이는 명령어를 사용하는데에 있어서 쿨타임을 적용하거나, 어떠한 명령을 실행하는데에 있어서 대기시간을 두는데에 사용하는 연산자입니다.

만약 내가 op 권한이 없는데, 10초를 기다리고 나서 스폰으로 텔레포트하게끔 기능을 구현하려면 어떻게 하는게 좋을까요?

@PLAYER 10초간 대기하십시오... - 명령을 실행한 플레이어에게 10초간 기다리라는 메시지를 출력하고

@PAUSE 10 - 다음 명령이 실행되기까지 작동을 10초 멈췄다가

@CMDOP spawn - op 의 권한이 있는 것으로 간주하여 /spawn 명령어가 실행된다

이와 같이 구현할 수 있습니다. @PAUSE 구문의 경우 범용성이 넓은데다가 초 단위로 시간을 지정해주기만 하면 비교적 제약없이 작동하기 때문에, 상당히 많은 비중을 차지하는 연산자라고 할 수 있습니다.

# 조건 연산자

- @IF [i|b|s] [$VARIABLE|CONSTANT] [=|!=|>|<|<=|>=\?=] [$VARIABLE|CONSTANT|true|false]  
- @AND [i|b|s] [$VARIABLE|CONSTANT] [=|!=|>|<|<=|>=\?=] [$VARIABLE|CONSTANT|true|false] 
- @OR [i|b|s] [$VARIABLE|CONSTANT] [=|!=|>|<|<=|>=\?=] [$VARIABLE|CONSTANT|true|false] 

코딩의 꽃이자 VariableTriggers 의 열매인 조건 연산자입니다. 써있는 것만 보면 대단히 복잡하고 어려워보이는데, 실제로도 트리거에서 가장 많은 오류를 배출하는 한편 가장 많은 기능을 구현할 수 있는 연산자라고 할 수 있습니다. 천천히, 그리고 자세히 뜯어보겠습니다.

[i|b|s] - 각각 integer, boolean, string 을 의미합니다. 코딩을 어느정도 해보신 분이라면 이해하시겠지만, 처음 보는 분들에게는 대단히 생소할 것 같습니다. 각각 정수, 참거짓판단, 문자열의 의미를 가지고 있습니다. 제시문의 형태를 정의한다고 생각하면 됩니다.

[$VARIABLE|CONSTANT] - 정수에는 VARIABLE 이, 참거짓판단과 문자열에는 CONSTANT 가 대응합니다.\ 각각 변수와 상수라는 의미를 가지고 있는데, 정수형을 선택하였을 경우에는 반드시 \$를 앞에 표기하게 되어 있는 것이 특징입니다. 제시문을 정의한다고 생각하면 됩니다.

[=|!=|>|<|<=|>=\?=] - 부등호의 표기입니다. =는 같다, != 는 같지 않다, \> 는 크다, \< 는 작다, \<= 는 작거나 같다, \>= 는 크거나 같다 에 대응하며, 일반적으로 ?= 는 사용되지 않습니다. 뒤 이어 나올 조건문과 제시문을 비교하기 위한 표기입니다.

[\$VARIABLE|CONSTANT|true|false] - 조건문을 나타냅니다. i에 \$VARIABLE, s에 CONSTANT, b에 true|false 가 대응합니다.

이러한 내용을 종합해 보면 다음과 같습니다.

i \$variable = \$variable

b constant = true|false

s constant = constant

이러한 양식에 따라서 작성되면 조건문이 완성되는 것입니다. 가장 어려운 내용이고 가장 중요한 부분이기에 여러가지 예시를 들며 설명을 해보겠습니다.

만약 임의의 정수값 playername.int 가 1 이라면, 이라는 코드를 어떻게 작성하면 될까요?

@IF i \<playername>.int = 1 - 임의의 정수값 \<playername.int> 가 1이라면

특정한 권한을 가지고 있는지를 검사하고 싶은데, 만약 studysulgi.mod 권한을 가지고 있다면, 이라는 코드는 어떻게 작성하면 될까요?

@IF b \<haspermission:studysulgi.mod> = true - studysulgi.mod 권한을 가진게 사실이라면

(여기서의 haspermission 은 아랫 문단에서 다룰 플레이스 홀더입니다만, 예시를 위해 어쩔 수 없이 제시합니다.)

만약 명령을 실행하는 플레이어의 이름이 studysulgi 라면, 이라는 코드는 어떻게 작성할 수 있을까요?

@IF s \<playername> = studysulgi - 명령을 실행하는 플레이어의 이름이 studysulgi 라면

(여기서의 \<playername> 은 아랫 문단에서 다룰 플레이스 홀더입니다만, 예시를 위해 어쩔 수 없이 제시합니다.)

@AND 와 @OR 은 @IF 연산자와 같이 사용되어야 하며, 조건을 추가할 떄 사용됩니다. 보통 첫번째 조건을 @IF 로 제시하고, @AND 나 @OR 로 이후의 조건을 제시하게 됩니다. @OR은 ~이거나, @AND 는 ~이고 의 조건문으로 사용되는데, @OR의 경우 여러가지 조건 중 하나만 만족하면 그 이후의 명령이 실행되는 반면 @AND로 추가된 조건은 모든 조건이 만족되어야 그 이후의 명령이 실행된다는 차이점이 있습니다.

@AND 와 @OR 은 @IF 의 문법을 그대로 준용하기 때문에 추가적인 설명 없이 넘어가겠습니다.

- @ELSE - @IF / @AND / @OR 로 작성된 조건이 부합되지 않았을 때 이후 명령이 실행되게 해줍니다.

가령 @IF 로 특정 권한이 있는지를 판별하는 조건을 걸어두었다면, @ELSE 를 작성함으로써 특정 권한이 없는 사람에게 실행될 명령을 지정할 수 있는 셈입니다. 이 @ELSE 연산자는 반드시 @IF 연산자가 선언된 이후에 사용해야 합니다.

- @EXIT - 스크립트에서 빠져나옵니다.

일반적으로 조건문 연산자를 모두 사용하고 조건문이 끝났다는 표시를 하기 이전에 사용합니다만, 실제로는 이후에 나오는 @ENDIF 만으로도 충분히 잘 작동하는 경향이 있어서 생략되는 경우가 많습니다.

- @ENDIF - 조건문 연산자를 종료합니다.

@IF 구문과 짝을 이루어서 사용되며, 조건문이 끝나고 명령문까지 모두 입력된 이후에 사용되어야 합니다. 예시를 들자면

@IF b \<haspermission:studysulgi.mod> = true - 만약 studysulgi.mod 권한이 있다면

@PLAYER You are a moderator! - 관리자라는 메시지 표시

@ELSE - 만약 studysulgi.mod 권한이 없다면

@PLAYER You are not a moderator! - 관리자가 아니라는 메시지 표시

@EXIT - 스크립트에서 빠져나오고 

@ENDIF - 조건문 연산자를 종료

이렇듯 @IF와 @ENDIF 는 늘 붙어 다니는 짝꿍과 같은 존재입니다. 일반적인 프로그래밍 언어들이 대부분 if와 중괄호를 이용해서 시작과 끝을 표시하는 것과 다르게 조건문을 끝내기 위한 연산자가 따로 존재합니다.

# 월드에 대한 연산자

- @SETBLOCK - 사용되지 않습니다.
- @SETBLOCKSAFE [BLOCKID:DATA] [x,y,z] - 지정된 좌표의 블록을 BLOCKID:DATA 로 Replacement 합니다. 거의 사용하지 않습니다.
- @TOGGLEBLOCK [BLOCKID:DATA] [x,y,z] - 지정된 좌표에 블록이 표시되거나 표시되지 않게 합니다.

- @DROPITEM [ITEMNAME] [QUANTITY] [ENCHANTMENTS] [x,y,z] - 지정된 좌표에 지정한 아이템이 나오게 합니다.

@DROPITEM 의 경우 간단한 RPG 아이템을 구현하는데에 있어서 도움이 될 수 있습니다만, 아이템의 이름이나 설명을 변경할 수 없기 때문에 사실 상 반쪽짜리 기능이나 다름이 없습니다.

아이템의 이름은 영문으로 표기해야 하고, 만약 띄어쓰기가 있는 경우에는 _ 를 이용해서 붙여줘야 제대로 작동합니다.

인챈트의 경우에는 띄어쓰기가 있더라도 _ 를 이용하지 않고 붙여서 써야 합니다. https://minecraftwiki.net/wiki/Enchanting 해당 페이지에 적혀있는 인챈트명을 그대로 쓰면 됩니다.

- @SIGNTEXT [x,y,z] [LINE] [TEXT] - 지정된 좌표에 놓여있는 표지판의 지정된 열의 텍스트를 replacement 합니다.

만약 좌표가 지정되어 있지 않다면, 해당 구문을 입력하였을 때 채팅창에 좌표를 묻는 질문이 나오고, 거기에 좌표를 입력해주면 됩니다. 또한 마인크래프트 내부에서는 컬러코드를 지원하고 있는데, [TEXT] 안에 컬러코드를 포함시키면 색상이 반영된 채로 텍스트가 수정됩니다.

- @FALLINGBLOCK - 사용되지 않습니다.
- @ENTITY [ENTITY] [QUANTITY] [x,y,z] - 지정한 좌표에 지정한 수량만큼의 엔티티를 소환합니다.

일반적으로 몹을 소환할 때는 스폰알을 이용하면 간편하지만, 트리거를 통해서 엔티티의 소환을 원한다면 해당 연산자를 이용할 수 있습니다. [ENTITY] 항목에는 영문으로 된 엔티티 이름이 들어가야 하며, 소환할 수 있는 엔티티의 종류는 다음과 같습니다:

BLAZE, CAVE_SPIDER, CHICKEN, COW, CREEPER, ENDER_DRAGON, ENDERMAN, EXPERIENCE_ORB, GHAST, GIANT, IRON_GOLEM, MAGMA_CUBE, MUSHROOM_COW, PIG, PIG_ZOMBIE, SHEEP, SILVERFISH, SKELETON, SLIME, SNOWMAN, SPIDER, SQUID, VILLAGER, FARMER, BLACKSMITH, BUTCHER, LIBRARIAN, PRIEST, WOLF, ZOMBIE

# 플레이어에 대한 연산자

- @TP [LOCATION] - 미리 지정해둔 장소로 명령을 실행한 플레이어를 이동합니다.

일반적으로 좌표를 입력받는 것과는 다르게, 이 연산자는 기존에 /vt setloc $ \<object>.variable 로 이동할 곳을 지정해두고 [LOCATION] 을 $ \<object>.variable 로 replacement 해야 작동합니다.

- @WORLDTP [playername] [world] 특정한 사용자를 특정한 이름의 월드로 이동시킵니다.

사실 상 거의 사용되지 않습니다. EssentialsX 플러그인이나 MultiWorld 플러그인과 같이 사용하는 경우에는 /world [world] 혹은 /mw move \<playername> [world] 로 쉽게 대체가 가능하기 때문입니다.

- @OPENINV - InventoryTriggers.yml 파일에 있는 GUI 형태의 트리거를 시작합니다.

일반적으로 버그가 많아서 거의 사용하지 않는다고 알려져있지만, 개인적인 생각으로는 구현을 하고 디버그를 하는게 다른 연산자에 비해서 고 난이도이기 때문에 다루는 사람들이 많이 없는 것으로 보여집니다. 이 부분에 대해서는 충분한 정보를 수집하고 난 뒤에 수정하도록 하겠습니다.

- @CLOSEINV - GUI 형태의 트리거를 강제 종료합니다.
- @MODIFYPLAYER [player] [modification] [value] - 지정된 플레이어의 상태를 value 로 변경합니다.

트리거를 이용하여 개발할 수 있는 영역 중 가장 범위가 넓은 부분입니다. [modification] 안에 들어갈 수 있는 옵션은 다음과 같습니다:

- HEALTH number
- FOOD number
- SATURATION number
- EXP and XP number
- WALKSPEED number, please note over 1 is insanely fast; use 0.2 for a little bit of boost.
- FLYSPEED number, please note over 1 is insanely fast; use 0.2 for a little bit of boost.
- DISPLAYNAME string, you can use color codes like &a
- LISTNAME string, you can use colors codes like &a
- FLYING boolean
- GAMEMODE string, use creative, survival, or adventure
- MAXHEALTH number
- HELDITEM string, use a valid material, like cobblestone
- HELDITEM:MATERIAL string, this keeps the item data and just swaps the material type
- HELDITEM:ID string, same as above but in ID form
- HELDITEM:META number, used to change the type of something such as white wool to pink wool
- HELDITEM:AMOUNT number up to 64
- HELDITEM:ENCHANT string, use a valid enchant like damage_all. This will always be a level 1 enchant
- HELDITEM:DISPLAYNAME string, keeps data but changes the item name. Color codes work.
- HELDITEM:LORE:SET string, sets the lore to that exactly.
- HELDITEM:LORE:ADD string, must already have lore to add to
- HELDITEM:LORE:REMOVE string, kills all lore
- HIDDEN boolean, hides (or unhides) the player from the server (invisible and in tab)
- BANNED boolean, this requires advanced mode, and won't kick the player from the server
- OPERATOR boolean, this required advanced mode and puts a message in console

재미있게도 이렇게 많은 modification 중 대부분이 EssnetialsX 플러그인으로 대체가 되기 때문에, 보통은 FLYING, GAMEMODE, MAXHEALTH, HELDITEM 정도의 modification 만 사용합니다. 이에 대한 자세한 설명을 덧붙입니다.

- FLYING boolean - 명령을 실행하고 있는 사람의 플라이의 가능 여부를 조정합니다.

이를테면 studysulgi.mod 권한을 가지고 있는 사람이라면 플라이를 키고, 그렇지 않다면 끄게 만들고 싶다면 다음과 같이 작성할 수 있습니다:

@IF b \<haspermission:studysulgi.mod> = true

@MODIFYPLAYER \<playername> FLYING true

@ELSE

@MODIFYPLAYER \<playername> FLYING false

@EXIT

@ENDIF

- GAMEMODE string - 명령을 실행하고 있는 사람의 게임모드를 조정합니다.

여기서 string 에 0,1,2,3 등 EssentialsX 처럼 숫자를 넣으면 작동하지 않고, 게임 모드의 정확한 명칭인 Survival, Creative, Spectator 등을 적어주어야 작동합니다.

- maxhealth number - 명령을 실행하고 있는 사람의 최대체력을 조정합니다.

마찬가지로 number 에 적은 숫자로 명령을 실행하는 사람의 최대체력을 바꿔줍니다. 기본적인 세팅 값은 20 이므로 이보다 더 크게하면 다른 플러그인이나 모드가 없어도 플레이어가 더 많은 체력을 가지게 할 수 있습니다.

- HELDITEM:ID - 플레이어가 들고 있는 아이템의 ItemID 입니다.
- HELDITEM:META - 플레이어가 들고 있는 아이템의 메타데이터입니다. 양털의 색상처럼 35:4 에서 4를 의미합니다.
- HELDITEM:AMOUNT - 플레이어가 들고 있는 아이템의 개수입니다. 최대치는 64입니다.
- HELDITEM:ENCHANT - 플레이어가 들고 있는 아이템에 특정 인챈트를 부여합니다.

일반적으로 ENCHANT의 구문은 사용되지 않습니다. 이유라면 어떤 인챈트를 부여하던 레벨이 1로 고정되기 때문에 실질적인 아이템을 만드는데에 있어서는 큰 의미가 없고, 고유 아이템을 만들때에 복사나 위조의 방지를 위해서 강제로 인챈트를 부여하는 기능 말고는 사용을 하는데에 있어서의 제약이 너무 많기 때문입니다.

- HELDITEM:DISPLAYNAME - 플레이어가 들고 있는 아이템의 이름을 변경합니다.
- HELDITEM:LORE:SET - 플레이어가 들고 있는 아이템의 설명을 최초 추가합니다.
- HELDITEM:LORE:ADD - 플레이어가 들고 있는 아이템의 설명을 추가합니다.

여기서 LORE:SET 과 LORE:ADD 의 차이점이라고 한다면, 반드시 ADD 보다 SET 이 먼저 작성되어야 한다는 점입니다. 만약 이 순서가 바뀐다면 SET으로 작성된 설명은 추가되지 않고 ADD로 작성된 설명 1줄만 추가됩니다.

- HELDITEM:LORE:REMOVE - 플레이어가 들고 있는 아이템의 설명을 모두 삭제합니다.

# 효과 연산자

이 글에서는 효과 연산자에 대해서는 다루지 않을 예정입니다. 효과 연산자의 경우 연산자 중에서도 가장 부수적인 연산자로 꼽히는데, 이러한 효과 연산자를 활용해야겠다고 마음을 먹은 경우 이 글을 참조하지 않아도 언제든지 방법을 찾을 수 있는 수준에 도달할 것으로 예상되기 때문입니다.

# 변수 수집 연산자

- @GETBLOCK [obj.var] [location] - 지정된 위치에 있는 블럭의 정보를 obj.var 에 저장합니다.
- @GETENTITYCOUNT [obj.var] [entity] [radius] - 명령을 실행한 사람의 설정된 반경에 설정된 엔티티의 수를 obj.var 에 저장합니다.
- @GETLIGHT - 사용하지 않습니다.

# 동적 변수 연산자

- @SETINT [obj.var] [int] - obj.var 의 숫자를 변경합니다.
- @ADDINT [obj.var] [int] - obj.var 에 지정한 숫자를 더합니다.
- @SUBINT [obj.var] [int] - obj.var 에 지정한 숫자를 뺍니다.
- @MULINT [obj.var] [int] - obj.var 에 지정한 숫자를 곱합니다.
- @DIVINT [obj.var] [int] - obj.var 에 지정한 숫자를 나눕니다.

트리거에서 구문이 사용되었는지를 확인하고 쿨타임을 정하거나, 어떠한 변수값을 저장할 때 유용하게 쓰이는 연산자입니다. 트리거 개발을 하는데에 있어서 중요도가 상당히 높습니다.

- @SETBOOL [obj.var] [true|false] - obj.var 의 참거짓판단을 설정합니다.
- @SETSTR [obj.var] [text] - obj.var 를 text 로 지정합니다.
- @ADDSTR [obj.var] [text] - obj.var 에 text 를 추가합니다.
- @GETSTRLEN [obj.var] [stringtotest] - stringtotest 의 글자 수를 확인하고 obj.var 에 저장합니다.
- @DELVAR - 사용하지 않습니다.
- @DELOBJ - 사용하지 않습니다.
- @STRBUILD - 사용하지 않습니다.

# 다음 파트로 넘어가기 전..

상당히 많은 파트로 전개되어 있는 것이 연산자 항목이지만, 그만큼 VariableTriggers 를 이용하여 트리거를 구현하는데에 있어서 빠질 수 없는게 연산자 항목입니다. 작성을 하고보니 중요도에 비해 설명이 부실한 파트도 있습니다만, 그런 부분에 대해서는 예제 코드 등을 추후 추가하도록 하겠습니다.

# 플레이스 홀더 \<>







# AreaTriggers
우리가 Minecraft 멀티플레이 서버를 열 때 많이 사용해본 플러그인 중 "WorldEdit" 이라는 플러그인이 있습니다. 아마 이 글을 읽어주시는 대부분의 분들은 아시는 플러그인일텐데, 이 플러그인은 나무 도끼를 완드로 이용하여 pos1, pos2 를 지정하고, 해당 위치에 대한 명령값을 입력하면 블럭이 replacement 되는 구조로 설계된 플러그인입니다.

VariableTriggers 내에 있는 이벤트중 Area, Click, Walk 의 이벤트를 다룰 때에도 "WorldEdit" 플러그인 처럼 특별한 완드가 필요한데, 월드에딧 플러그인과는 다르게 뼈 아이템을 사용합니다. 따라서 이 글에서 다루는 내용들을 수행하기 위해서는 반드시 뼈 아이템을 1개 이상 가지고 있어야 합니다.

채팅창에 /vt setarea 라고 입력하게 되면, 가지고 있던 뼈 아이템을 완드로 이용하여 pos1, pos2 를 지정할 수 있게 됩니다. 일반적으로 pos1은 지정하고자 하는 영역의 왼쪽 윗 꼭짓점, pos2는 지정하고자 하는 영역의 오른쪽 아랫 꼭짓점을 선택하긴 합니다만, 이는 내가 어떠한 트리거를 지정하기 위한 영역을 설정하기만 하면 되므로 전혀 상관이 없습니다.

pos1과 pos2가 지정되면, /vt definearea [areaname] 을 입력함으로써 지정된 영역을 [areaname] 으로 정의할 수 있습니다. 이 때 실제 명령어를 입력할 때에는 [] 는 빼고, areaname 은 자신이 원하는 영역의 이름으로 적으면 AreaTriggers 를 이용할 준비가 완료됩니다.

AreaTriggers 는 두 가지의 가장 큰 조건을 가지고 있습니다.
- 지정된 영역에 들어왔는가? (ENTER)
- 지정된 영역에서 나갔는가? (EXIT)

즉 내가 지정한 영역에 들어왔을 때 수행할 명령과, 내가 지정한 영역에서 나갔을 때 수행할 명령을 지정할 수 있는 트리거라고 보면 됩니다. 가령 누군가가 내가 spawnpoint 라고 설정해놓은 영역에 들어오면 어서 오세요! 라고, 이 영역에서 나갔을 때는 안녕히 가세요! 라고 출력하고자 한다면:

/vt setarea - pos1, pos2 지정

/vt definearea spawnpoint - pos1, pos2 를 통해 지정된 영역을 spawnpoint 로 정의

/vta spawnpoint ENTER @PLAYER 어서 오세요! - spawnpoint 로 정의된 영역에 들어왔을 경우 들어온 플레이어에게 어서 오세요! 라는 메세지를 출력하고,

/vta spawnpoint EXIT @PLAYER 안녕히 가세요! - spawnpoint 로 정의된 영역에서 나갔을 경우 나간 플레이어에게 안녕히 가세요! 라는 메세지를 출력함.

이렇게 4줄의 명령어를 입력함으로써 새로운 AreaTriggers 를 생성하는데에 성공한 것입니다.

한 가지의 예제를 더 수행해보겠습니다. 만약 내가 DeathZone 이라고 설정해놓은 영역에 들어가면 접속하고 있는 모든 플레이어 중 랜덤으로 한 명을 죽게 만들고, 이 영역에서 나갔을 때는 접속하고 있는 모든 플레이어를 죽게 만들고자 한다면 어떻게 구현하는게 좋을까요?

/vt setarea - pos1, pos2 지정

/vt definearea DeathZone - pos1, pos2 를 통해 지정된 영역을 DeathZone 으로 정의

/vta DeathZone ENTER @CMDOP minecraft:kill @r - DeathZone 으로 정의된 영역에 들어오면 랜덤한 플레이어 한 명을 죽게 하고,

/vta DeathZone EXIT @CMDOP minecraft:kill @a - DeathZone 으로 정의된 영역에서 나가면 모든 플레이어를 죽게 함.

이번에도 4줄의 명령어를 입력함으로써 내가 원하는 기능을 구현하는데에 성공한 것입니다.

이렇듯 VariableTriggers 플러그인은 물론이고, 코딩은 상당히 직관적입니다. 내가 원하는 기능을 구현하고자 한다면 우선 그 기능이 실행되기 위한 구조를 생각해보고, 그 구조를 그대로 코딩을 통해 만들어주기만 하면 됩니다.






