<script>

    import CollapsibleCard from "../components/CollapsibleCard.svelte";

    let isTimetableOpen = false;
    function onTimetableToggle() {
        isTimetableOpen = !isTimetableOpen;
    }
    function goToLogin() {
        window.location.href = '/login';
    }
</script>
<section id="sidebar">
    <div id="profile-info">
        <img src="assets/logo.svg" alt="profile">
        <p class="stdnumber">20221000</p>
        <p class="name">홍길동</p>
    </div>
    <div id="profile-buttons">
        <button>
            <i class="fa-regular fa-bookmark"></i><br>
            내 정보
        </button>
        <button>
            <i class="fa-solid fa-right-from-bracket"></i><br>
            로그아웃
        </button>
    </div>
    <div id="timetable">
        <CollapsibleCard>
            <div slot="header" style="margin-top: 24px">
                <hr>
                <p>내 시간표</p>
            </div>
            <div slot="body">
                <img src="assets/timetable_dummy.png" alt="timetable">
            </div>
        </CollapsibleCard>
        <div id="next-class">
            <i class="fa-regular fa-clock"></i>
            <div>
                <p id="class-name">웹프로그래밍</p>
                <p id="class-time">오늘 10:30 ~ 12:30 | 아)2공 509</p>
            </div>
        </div>
    </div>
</section>
<section id="main">

</section>