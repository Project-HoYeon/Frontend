<script>

    let colorIdx = 0;
    const colors = ["#f18577", "#e9c16a", "#a6c870", "#96B6C5", "#7fd1c2", "#fca965", "#9d84e2", "#7acb87", "#d195ea"];

    /** @type {Classes} */
    const classes = [
        [ // 9
            { name: "테스트", pos: "2공410", span: 2 },
            { name: "하기싫다", pos: "조형211", span: 1 },
            null,
            { name: "하기싫다", pos: "조형211", span: 1 },
            null,
            { name: "하기싫다", pos: "조형211", span: 1 },
        ],
        [ // 10
            null,
            null,
            { name: "뷁", pos: "1공432", span: 2 }
        ],
        [ // 11

        ],
        [ // 12

        ],
        [ // 13

        ],
        [ // 14

        ],
        [ // 15
            null,
            { name: "뷁", pos: "1공432", span: 2 },
        ],
        [ // 16

        ],
        [ // 17

        ],
        [ // 18

        ],
    ];

    /** @returns string */
    function getNextColor() {
        return colors[colorIdx >= colors.length ? (() => { colorIdx = 0; return colorIdx; })() : colorIdx++];
    }

    /**
     * @typedef {(Class|null)[][]} Classes
     * @typedef {{name: string, pos: string, span: number}} Class
     */
</script>
<div class="wrapper">
    <table>
        <tr class="row head">
            <th></th>
            <th>월</th>
            <th>화</th>
            <th>수</th>
            <th>목</th>
            <th>금</th>
            <th>토</th>
        </tr>
        {#each classes as hour, idx}
            <tr class="row body">
                <td class="hour">{9 + idx}</td>
                {#each hour as clazz}
                    {#if clazz}
                        <td rowspan="{clazz.span}" style="background-color: {getNextColor()}">{clazz.name}<br><span>{clazz.pos}</span></td>
                    {:else}
                        <td class="empty"></td>
                    {/if}
                {/each}
            </tr>
        {/each}
    </table>
</div>
<style lang="scss">
  $table-width: 22rem;
  $cell-width: 3.5rem;

  .wrapper {
    display: flex;
    justify-content: center;

    table {
      min-width: $table-width;
    }
  }

  .row {
    width: $table-width;

    &.body {
      height: 2.5rem;

      & > td:not(.hour) {
        width: $cell-width;
        font-size: 0.8rem;
        font-weight: 700;
        vertical-align: top;
        text-align: center;
        padding-top: 0.4rem;
        background-color: #6a87ce;
        color: white;

        & > span {
          color: #efefef;
          font-size: 0.75rem;
          font-weight: 500;
        }

        &.empty {
          background-color: transparent;
        }
      }

      & > td.hour {
        width: 1.5rem;
      }
    }
  }
</style>