


<script>
  

    
    const data = [
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
        }

    ]
    // @ts-ignore
    import { onMount } from 'svelte';


    let year = new Date().getFullYear();
    let month = new Date().getMonth() + 1; 
    let schedule = getSchedule(year, month);

    
    onMount(() => {
         schedule = getSchedule(year, month);
    });

    // @ts-ignore
    /**
   * @type {any[]}
   */
    let calendarDays;

   // @ts-ignore
    // @ts-ignore
     // @ts-ignore
      // @ts-ignore
       // @ts-ignore
        // @ts-ignore
          $: {
        calendarDays = generateCalendarDays(year, month);
    }

    // @ts-ignore
    function generateCalendarDays(year, month) {
        let days = [];
        const firstDayOfMonth = new Date(year, month - 1, 1);
        const firstDayIndex = firstDayOfMonth.getDay();
        const lastDayOfMonth = new Date(year, month, 0).getDate();

        // @ts-ignore
        // @ts-ignore
        

        for (let i = 0; i < firstDayIndex; i++) {
            days.push({
                day : "",
                check : false,
            });
        }
      
    
        
        for (let day = 1; day <= lastDayOfMonth; day++) {
        let eventExists = schedule.some(event => {
            const [startStr, endStr] = event.date.split(' ~ ');
            const startDate = new Date(startStr);
            const endDate = new Date(endStr);
            return new Date(year, month-1, day + 1) >= startDate && new Date(year, month-1, day) <= endDate;
        });
        days.push({ day, check: eventExists });
    }
        return days;
    }
   
 
    // @ts-ignore
    function getSchedule(year, month){
         const date = data.filter((item)=> {
            const [start, end] = item.date.split(' ~ ').map(d => new Date(d));
            return ((start.getMonth() + 1 === month) && (start.getFullYear() === year) || 
                    ((end.getMonth() + 1 === month)  && end.getFullYear() == year))
        })
       return date;
        
    }
   
    // @ts-ignore
    function changeYear(direction){

        year += (direction === 'up' ? 1 : -1);
        year = Math.max(2023, Math.min(year, 2024));
        schedule = getSchedule(year, month);
    }
    
   
    // @ts-ignore
    function changeMonth(direction){
        
        month += (direction === 'up' ? 1 : -1);
        if (month > 12) { month = 1; year++; }
        else if (month < 1) { month = 12; year--; }
        year = Math.max(2023, Math.min(year, 2024));
        schedule = getSchedule(year, month);
    }
    

const maxVisible = 5;

let visibleCount = maxVisible;
// @ts-ignore

$: visibleSchedule = schedule.slice(0, visibleCount);

function showMore() {
    visibleCount = schedule.length;
   
}

</script>
<section>
    <div class="left-container">
        <div class="controls">
            <div class="year-display">  
                <button class="arrow" on:click={() => changeYear('up')}><i class="fas fa-angle-up" style="color: #007bff; "></i></button>
                <div class="date">{year}년</div>
                <button class="arrow" on:click={() => changeYear('down')}><i class="fas fa-angle-down" style="color : #007bff; "></i></button>
            </div>
            <div class="month-display">
                <button class="arrow" on:click={() => changeMonth('up')}><i class="fas fa-angle-up" style="color : #007bff; "></i></button>
                <div class="date">{month}월</div>
                <button class="arrow" on:click={() => changeMonth('down')}><i class="fas fa-angle-down" style="color : #007bff;"></i></button>
            </div>
        </div>
        <div class="calendar">
            <div class="calendar-grid">
                {#each calendarDays as item}
                    <div class="calendar-day" style ="background-color : {item.check ? "#007bff;" : "#f9f9f9"};" >{item.day}</div>
                {/each}
            </div>
        </div>
    </div>
    <div class="right-container">
        <h1 class = "main_title">학사일정</h1>

        <ul>
            {#each visibleSchedule  as item, index (item.date + '-' + index)}
            <li><strong>{item.date}</strong>: <sapn>{item.context}</sapn></li>
            {/each}
            {#if schedule.length > visibleCount}
            <button on:click={showMore} class="show-more" >더보기</button>
            {/if}
        </ul>
        
    </div>
</section>

<style>
    section {
    position : absolute;
    display: flex;
    align-items: start;
    justify-content: center;
    box-sizing: border-box;
    width: 100%;
    height : 100%;
    background-color: #f5f5f5;
    padding: 20px;
}

.left-container, .right-container {
    display : flex;
    flex-direction: column;
    align-items: center;
    flex: 1;
    margin: 40px;
    background-color: #fff;
    padding: 20px;
    border-radius: 10px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);

}

.main_title {
    font-size: 2.5rem;
    color: #333;
    text-align: center;
    margin-bottom: 20px;
}

.controls {
    display: flex;
    justify-content: center;
    padding: 10px;
    margin-bottom: 20px;
}

.year-display, .month-display {
    display: flex;
    flex-direction: column;
    align-items: center;
}

.arrow {
    cursor: pointer;
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 2rem;
    background-color: white;
    border: none;
    border-radius: 0.5rem;
}

.date {
    font-size: 1.5rem;
    font-weight: bold;
    margin: 0.5rem;
}

.calendar {
    max-width: 350px;
    background-color: #fff;
    border-radius: 10px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    padding: 20px;
    text-align: center;
}

.calendar-grid {
    display: grid;
    grid-template-columns: repeat(7, 1fr);
    gap: 5px;
}

.calendar-day {
    padding: 10px;
    border-radius: 5px;
    background-color: #f9f9f9;
    color: #333;
    font-weight: bold;
    transition: background-color 0.2s ease-in-out;
}



ul {
    list-style: none;
    margin-top: 1rem;
}

li {
    background-color: #fff;
    margin-bottom: 0.5rem;
    padding: 0.75rem;
    border-radius: 0.5rem;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    transition: background-color 0.2s;
}

li:hover {
    background-color: #e9ecef;
}

li strong {
    color: #007bff;
    margin-right: 0.5rem;
}

.show-more {
    padding: 0.5rem 1rem;
    font-size: 1rem;
    color: #fff;
    background-color: #007bff;
    border: none;
    border-radius: 0.3rem;
    margin-top: 0.5rem;
    cursor: pointer;
    transition: background-color 0.2s;
}

.show-more:hover {
    background-color: #0056b3;
}

</style>




