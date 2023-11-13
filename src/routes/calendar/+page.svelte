<script>
    import "./styles.scss";

    /** @type Schedules */
    const data = [
        {
            date : "2023-02-06 ~ 2023-02-10",
            context : "1학기 수강신청 기간"
        },
        {
            date : "2023-02-13 ~ 2023-02-17",
            context :"1학기 등록 및 일반휴학 접수 기간",
        },
        {
            date : "2023-02-27 ~ 2023-02-27",
            context : "신입생 수강신청"
        },
        {
            date : "2023-02-27 ~ 2023-02-27",
            context : "2023학년도 1학기 개강"
        },
        {
            date : "2023-06-01 ~ 2023-06-29",
            context : "성적입력 기간"
        },
        {
            date : "2023-06-15 ~ 2023-06-15",
            context : "학기종료일"
        },
        {
            date : "2023-06-16 ~ 2023-06-16",
            context : "하계방학"
        },
        {
            date : "2023-06-19 ~ 2023-07-07",
            context : "하계계절학기"
        },
        {
            date : "2023-06-22 ~ 2023-07-05",
            context : "1학기 강의평가(수업반응도) 기간"
        },
        {
            date : "2023-07-03 ~ 2023-07-05",
            context : "성적정정 기간"
        },
        {
            date : "2023-07-31 ~ 2023-08-04",
            context : "2학기 복학원서 접수 기간"
        },
        {
            date : "2023-08-16 ~ 2023-08-21",
            context : "2학기 일반휴학 접수기간: 16(수)~18(금), 21(월)"
        },
        {
            date : "2023-08-16 ~ 2023-08-25",
            context : "2학기 등록기간"
        },
        {
            date : "2023-08-16 ~ 2023-08-22",
            context : "2학기 수강신청 기간: 16(수)~18(금), 21(월)~22(화)"
        },
        {
            date : "2023-08-18 ~ 2023-08-18",
            context : "제39회 후기 학위수여식"
        },
        {
            date : "2023-08-28 ~ 2023-08-28",
            context : "2023학년도 2학기 개강"
        },
        {
            date : "2023-09-26 ~ 2023-09-26",
            context : "수업 30일선(휴업일 포함)"
        },
        {
            date : "2023-09-28 ~ 2023-09-28",
            context : "개교기념일"
        },
        {
            date : "2023-10-04 ~ 2023-10-04",
            context : "학기 1/3선"
        },
        {
            date : "2023-10-11 ~ 2023-10-11",
            context : "수업 45일선(휴업일 포함)"
        },
        {
            date : "2023-10-24 ~ 2023-10-24",
            context : "학기 1/2선"
        },
        {
            date : "2023-11-10 ~ 2023-11-10",
            context : "학기 2/3선"
        },
        {
            date : "2023-12-01 ~ 2023-12-28",
            context : "성적입력 기간 : 12.01.(금) ~ 12.28.(목)"
        },

        {
            date : "2023-12-18 ~ 2023-12-18",
            context : "학기종료일"
        },
        {
            date : "2023-12-19 ~ 2023-12-19",
            context : "동계방학"
        },
        {
            date : "2023-12-20 ~ 2024-01-09",
            context : "동계계절학기: 12.20.(수) ~ 2024.01.09.(화)"
        },
        {
            date : "2023-12-21 ~ 2024-01-04",
            context : "2학기 강의평가(수업반응도) 기간 : 12.21.(목) ~ 2024.01.04.(목)",
        },
        {
            date : "2024-01-02 ~ 2024-01-04",
            context : "성적정정 기간: 01.02(화) ~ 01.04.(목)"
        },
        {
            date : "2024-01-02 ~ 2024-01-02",
            context : "신년예배 및 하례식"
        },
        {
            date : "2024-01-08 ~ 2024-01-12",
            context : "1학기 복학원서 접수 기간: 01.08.(월) ~ 01.12.(금)"
        },
        {
            date : "2024-02-01 ~ 2024-02-07",
            context : "1학기 수강신청 기간: 02.01.(목) ~ 02.07.(수)"
        },
        {
            date : "2024-02-13 ~ 2024-02-16",
            context : "1학기 등록 및 일반 휴학원서 접수 기간: 02.13.(화) ~ 02.16.(금)"
        },
        {
            date : "2024-02-16 ~ 2024-02-16",
            context : "제40회 전기 학위수여식"
        },
        {date : "2024-02-21 ~ 2024-02-22",
            context : "편입생/재입학생 수강신청 기간: 02.21.(수) ~ 02.22.(목)"
        },
        {
            date : "2024-02-23 ~ 2024-02-23",
            context : "제44회 입학식"
        },
        {
            date : "2024-02-23 ~ 2024-02-23",
            context : "신입생 수강신청"
        },
        {
            date : "2024-02-26 ~ 2024-02-26",
            context : "2024학년도 1학기 개강"
        },
    ];

    const maxVisible = 5;
    let nowVisible = maxVisible;

    let year = new Date().getFullYear();
    let month = new Date().getMonth() + 1;

    /** @type Schedules */
    let schedules;
    /** @type CalendarDates */
    let calendarDates;
    /** @type Schedules */
    let visibleSchedules;

    /**
     * Generate date objects of month in calendar
     * @param {number} year
     * @param {number} month
     */
    function generateCalendarDays(year, month) {
        const days = [];
        const firstDayIndex = new Date(year, month - 1, 1).getDay();
        const lastDayOfMonth = new Date(year, month, 0).getDate();

        for (let i = 0; i < firstDayIndex; i++) {
            days.push({
                day : "",
                check : false,
            });
        }

        for (let day = 1; day <= lastDayOfMonth; day++) {
            let eventExists = schedules.some(event => {
                const [startStr, endStr] = event.date.split(' ~ ');
                const startDate = new Date(startStr);
                const endDate = new Date(endStr);
                return new Date(year, month - 1, day + 1) >= startDate && new Date(year, month - 1, day) <= endDate;
            });
            days.push({ day, check: eventExists });
        }
        return days;
    }

    /**
     *
     * @param {number} year
     * @param {number} month
     */
    function getSchedulesOf(year, month){
        return data.filter((item) => {
            const [start, end] = item.date
                .split(' ~ ')
                .map(d => new Date(d));

            return ((start.getMonth() + 1 === month) && (start.getFullYear() === year) ||
                ((end.getMonth() + 1 === month) && end.getFullYear() === year));
        });
    }

    function nextYear() {
        updateYear(1);
    }

    function prevYear() {
        updateYear(-1);
    }

    /**
     * Update year of calendar
     * @param {number} diff Difference from year now
     */
    function updateYear(diff) {
        year = Math.max(2023, Math.min(year + diff, 2024));
        updateSchedule();
    }

    function nextMonth() {
        updateMonth(1);
    }

    function prevMonth() {
        updateMonth(-1);
    }

    /**
     *
     * @param {number} diff
     */
    function updateMonth(diff) {
        if ((month += diff) > 12) {
            month = 1;
            year++;
        }
        else if (month < 1) {
            month = 12;
            year--;
        }
        year = Math.max(2023, Math.min(year, 2024));
        updateSchedule();
    }

    /**
     *
     * @param {boolean} [resetVisible]
     */
    function updateSchedule(resetVisible) {
        if (resetVisible)
            nowVisible = maxVisible;
        schedules = getSchedulesOf(year, month);
        calendarDates = generateCalendarDays(year, month);
        visibleSchedules = schedules.slice(0, nowVisible);
    }

    function showMore() {
        nowVisible = schedules.length;
        updateSchedule(false);
    }

    updateSchedule();

    /**
     * @typedef {Schedule[]} Schedules
     *
     * @typedef {{date: string, context: string}} Schedule
     *
     * @typedef {CalendarDate[]} CalendarDates
     *
     * @typedef {{day: number|string, check:boolean}} CalendarDate
     */
</script>
<section>
    <div class="container">
        <div class="controls">
            <div class="year-display">
                <button class="arrow" on:click={nextYear}><i class="fas fa-angle-up" style="color: #007bff; "></i></button>
                <div class="date">{year}년</div>
                <button class="arrow" on:click={prevYear}><i class="fas fa-angle-down" style="color : #007bff; "></i></button>
            </div>
            <div class="month-display">
                <button class="arrow" on:click={nextMonth}><i class="fas fa-angle-up" style="color : #007bff; "></i></button>
                <div class="date">{month}월</div>
                <button class="arrow" on:click={prevMonth}><i class="fas fa-angle-down" style="color : #007bff;"></i></button>
            </div>
        </div>
        <div class="calendar">
            <div class="calendar-grid">
                {#each calendarDates as date}
                    <div class="calendar-day" class:highlighted={date.check}>{date.day}</div>
                {/each}
            </div>
        </div>
    </div>
    <div class="container">
        <h1 class="main-title">학사일정</h1>
        <ul>
            {#if visibleSchedules === undefined}
                <p>로드중...</p>
            {:else}
                {#each visibleSchedules as schedule, index (schedule.date + '-' + index)}
                    <li><strong>{schedule.date}</strong>: <span>{schedule.context}</span></li>
                {/each}
            {/if}
            {#if schedules.length > nowVisible}
                <button on:click={showMore} class="show-more" >더보기</button>
            {/if}
        </ul>
    </div>
</section>