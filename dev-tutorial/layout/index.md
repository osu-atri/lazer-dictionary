---
enableComments: true
---

# 布局与界面设计

osu!lazer 的布局框架究竟是个什么样子，沐雨酱搞了两个多月，也并没有完全搞清楚。可能用 FillFlowContainer 与相对坐标的思路比较好，搞了个大舞台，可惜后来冒烟起火了...

...大型纪录片：《将设计进行到底》，正在播出...

:::note 美味制作中😋

大家好啊，我是说的占位符。

今天还没有大家想看的东西，有了之后一定会给你推过来的。

:::

```csharp
using System.Drawing;
using osu.Framework.Graphics.Containers;
using osu.Framework.Graphics;
using osu.Framework.Allocation;
using osuTK.Graphics;
using osu.Framework.Extensions.Color4Extensions;
using osu.Game.Graphics;
using osu.Framework.Bindables;
using osu.Framework.Graphics.Sprites;
using osuTK;

namespace osu.Game.Tournament.Components
{
    public partial class WindowSizeIndicator : CompositeDrawable
    {
        private BindableSize sizeBindable = new BindableSize();

        private TournamentSpriteText winWidthText = null!;
        private TournamentSpriteText winHeightText = null!;

        public WindowSizeIndicator(BindableSize bSize)
        {
            Anchor = Anchor.BottomRight;
            Origin = Anchor.BottomRight;

            sizeBindable = bSize;
            sizeBindable.BindValueChanged(bindSizeChanged);
        }

        [BackgroundDependencyLoader]
        private void load()
        {
            Width = 150;
            Height = 100;
            Alpha = 0;
            AlwaysPresent = true;

            InternalChildren = new Drawable[]
            {
                new EmptyBox(cornerRadius: 10)
                {
                    RelativeSizeAxes = Axes.Both,
                    Colour = Color4.Black.Opacity(0.6f),
                },
                new FillFlowContainer
                {
                    RelativeSizeAxes = Axes.Both,
                    Direction = FillDirection.Vertical,
                    Spacing = new Vector2(5),
                    Children = new Drawable[]
                    {
                        new FillFlowContainer
                        {
                            Height = 40,
                            Direction = FillDirection.Horizontal,
                            Spacing = new Vector2(5),
                            Children = new Drawable[]
                            {
                                new SpriteIcon
                                {
                                    Icon = FontAwesome.Solid.RulerHorizontal,
                                    Size = new Vector2(24),
                                },
                                winWidthText = new TournamentSpriteText
                                {
                                    Text = sizeBindable.Value.Width.ToString(),
                                    Colour = TournamentGame.TEXT_COLOUR,
                                    Font = OsuFont.Torus.With(size: 20, weight: FontWeight.SemiBold),
                                },
                            }
                        },
                        new FillFlowContainer
                        {
                            Height = 40,
                            Direction = FillDirection.Horizontal,
                            Spacing = new Vector2(5),
                            Children = new Drawable[]
                            {
                                new SpriteIcon
                                {
                                    Icon = FontAwesome.Solid.RulerVertical,
                                    Size = new Vector2(24),
                                },
                                winWidthText = new TournamentSpriteText
                                {
                                    Text = sizeBindable.Value.Height.ToString(),
                                    Colour = TournamentGame.TEXT_COLOUR,
                                    Font = OsuFont.Torus.With(size: 20, weight: FontWeight.SemiBold),
                                },
                            }
                        },
                    }
                },
            };
        }

        private void bindSizeChanged(ValueChangedEvent<Size> e)
        {
            winWidthText.Text = e.NewValue.Width.ToString();
            winHeightText.Text = e.NewValue.Height.ToString();
        }
    }
}
```
