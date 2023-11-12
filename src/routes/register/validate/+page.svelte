<script>
    import "./styles.scss";
    import {onMount} from "svelte";

    let title = "확인중..";
    let content = "요청이 유효한지 확인중이에요. 잠시만 기다려주세요 :D"

    onMount(() => {
        const queryString = window.location.search;
        const urlParams = new URLSearchParams(queryString);
        if (!urlParams.has("id")) {
            alert("잘못된 접근입니다.");
            goBack();
            return;
        }

        const requestID = urlParams.get("id") ?? "";
        if (!/^[0-9a-f]{8}-[0-9a-f]{4}-[0-5][0-9a-f]{3}-[089ab][0-9a-f]{3}-[0-9a-f]{12}$/i.test(requestID)) {
            alert("잘못된 접근입니다.");
            goBack();
            return;
        }

        fetch("https://server.mooner.dev/hoyeon/api/v1/auth/validate?id=" + requestID)
            .then(async v => {
                return {
                    code: v.status,
                    message: await v.text(),
                };
            })
            .then(resp => {
                if (resp.code !== 200) {
                    title = "이런!";
                    content = `서버와의 통신이 영 좋지 못했어요. 값을 확인하거나 나중에 다시 시도해주세요. (code: ${resp.code}, response: ${resp.message})`;
                } else {
                    title = "인증 성공!";
                    content = "이제 이 창을 닫고 가입 페이지로 돌아가 회원가입을 완료해 주세요 (˵ •̀ ᴗ - ˵ ) ✧";
                }
            })
    });

    function goBack() {
        window.history.go(-1);
    }
</script>
<section id="body">
    <h1>{title}</h1>
    <p>{content}</p>
</section>