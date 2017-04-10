블록(Block)
======

블록은 명백히 마인크래프트 세계에 필수적인 존재로서 각종 지형지물 및 기계 구조물을 구성합니다. 모드 제작에 관심이 있다면, 아마 블록을 추가하고 싶으시겠지요. 그런 만큼 현재 문서는 블록 제작법에 대해 안내하고, 이를 이용해 무엇을 할 수 있는지에 관해 다루어 볼 것입니다.

블록 제작하기
----------------

### 기본적인 블록들

특별한 기능을 요하지 않는 간단한 블록들(조약돌, 목재 등)은 굳이 새로운 클래스를 제작하지 않더라도 추가할 수 있습니다. 단순히 Block 클래스에 대한 인스턴스를 생성한 후 몇몇 설정 메소드들을 호출하는 방식으로, 서로 다른 유형의 블록들을 원하는 만큼 많이 추가할 수 있습니다. 설정 메소드들에는 다음과 같은 것들이 있습니다:

- `setHardness` - 블록을 깨는 데 걸리는 시간(경도)을 설정합니다. 단위는 임의로 정해져 있으니, 돌은 1.5, 흙은 0.5의 값을 갖는다는 것을 참고하세요. 편의를 위해, 블록을 부술 수 없게 만드는 `setBlockUnbreakable` 메소드가 제공됩니다.
- `setResistance` - 블록의 폭발 저항력을 설정합니다. 경도와는 별개의 속성이지만, `setHardness`메소드는 폭발 저항력이 경도 값의 5배보다 작은 경우 이 값으로 설정합니다.
- `setSoundType` - 블록을 때리거나, 캐거나, 설치할 때 나는 소리를 설정합니다. 인자로서 `SoundType`을 필요로 하는데, 이에 관해서는 [sounds] 문서를 참조해 주시기 바랍니다.
- `setLightLevel` - 블록이 발산하는 빛의 양을 설정합니다. **참고:** 이 메소드는 0부터 1의 값을 갖습니다. (0부터 15가 아닙니다) 이 값을 계산하기 위해서는, 블록이 발산할 밝기 레벨 값을 정하고 16으로 나누면 됩니다. 예를 들어, 밝기 레벨 5만큼의 빛을 발산하는 블록은 이 메소드에 `5 / 16f`의 값을 인자로 넘겨주어야 합니다.
- `setLightOpacity` - 빛이 이 블록을 통과하면서 밝기 레벨이 얼마나 줄어들지를 설정합니다. `setLightLevel`과는 달리 이 값은 0부터 15까지의 범위를 갖습니다. 예를 들어, 값을 `3`으로 설정한 경우, 이 블록을 통과할 때마다 밝기 레벨이 3씩 감소하게 됩니다.
- `setUnlocalizedName` - 번역 파일에 쓰일 이름을 설정합니다. 번역을 위해, 이 이름의 앞에는 'tile.'가 붙게 되고 끝에는 '.name'이 붙게 됩니다. **참고:** 사용자가 지정하는 unlocalizedName은 'tile.'이나 '.name'을 포함하지 않아야 합니다. 예를 들어 `setUnlocalizedName("block_of_charcoal")`의 실제 unlocalizedName 값은 "tile.block_of_charcoal.name"이 될 것입니다. 번역에 대한 심화 내용은 뒤에 다시 설명하게 될 겁니다.
- `setCreativeTab` - 어떤 크리에이티브 탭에 이 블럭이 위치할 것인지를 정합니다. 만약 이 블럭이 크리에이티브 탭에 표시되어야 한다면 이 과정을 반드시 거쳐야 합니다. 크리에이티브 탭의 목록은 'CreativeTabs' 클래스에서 찾을 수 있습니다.

이 모든 메소드는 *연속됩니다*. 이것은 메소드들을 순서대로 모두 각각 지정할 수 있다는 것입니다. `Block#registerBlocks`를 참고해 주십시오.

### Advanced Blocks

Of course, the above only allows for extremely basic blocks. If you want to add functionality, like player interaction, a custom class is required. However, the `Block` class has many methods and unfortunately not every single one can be documented here. See the rest of the pages in this section for things you can do with blocks.

Registering a Block
-------------------

So now that you have this block you've created, it's time to actually put it in the game. Thankfully this is extremely simple to do.

As with most things, blocks can be registered using `GameRegistry.register(...)`. For this method your block must have a "registry name" which is just another way of saying "unique name". The preferred method to set your block's registry name is by calling `setRegistryName` on it inside the call to `register`. For instance, `GameRegistry.register(myBlock.setRegistryName("foo"))`.

!!! note
    When using a simple string, the currently active mod's ID will be added. So if I was doing this from "mymod", the real registry name would be "mymod:foo".

It is also possible to pass a `ResourceLocation` directly into `register`, but this is just convenience for calling `setRegistryName` with the `ResourceLocation` beforehand.

Further Reading
---------------

For information about block properties, such as those used for vanilla blocks like wood types, fences, walls, and many more, see the section on [blockstates].

[sounds]: ../effects/sounds.md
[blockstates]: ../blockstates/states.md
