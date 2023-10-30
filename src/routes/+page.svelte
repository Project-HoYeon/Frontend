<script>

    import CollapsibleCard from "../components/CollapsibleCard.svelte";

    let isTimetableOpen = false;
    function onTimetableToggle() {
        isTimetableOpen = !isTimetableOpen;
    }
</script>
<section>
    <div id="sidebar">
        <div id="profile-info">
            <img src="assets/hoseo_logo_1.png" alt="profile">
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
        </div>
    </div>
</section>