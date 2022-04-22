<script>
    import pako from "pako";
    import { readable } from "svelte/store";
    export let params = {};
    function decode(data) {
        return JSON.parse(
            pako.inflate(
                window
                    .atob(data)
                    .split(",")
                    .map((x) => parseInt(x)),
                { to: "string" }
            )
        );
    }

    let data;
    try {
        data = decode(params.content);
    } catch (e) {
        data = undefined;
    }

    let height = 2012 * (window.innerWidth / 1078);

    window.onresize = () => {
        height = 2012 * (window.innerWidth / 1078);
    };

    function formatted() {
        const date = new Date();
        return `${date.getFullYear()}-${
            date.getMonth() + 1
        }-${date.getDate()} ${date.getHours()}:${date.getMinutes()}:${date.getSeconds()}`;
    }

    const time = readable(undefined, (set) => {
        set(formatted());
        const itv = setInterval(() => {
            set(formatted());
        }, 1000);

        return () => {
            clearInterval(itv);
        };
    });
</script>

{#if data === undefined}
    <h1>数据无效</h1>
{:else}
    <div class="bg">
        <div
            class="bg"
            class:leave={data.type === "leave"}
            class:back={data.type === "back"}
            style:height={height + "px"}
        >
            <p class="class">{data.class}</p>
            <p class="leave_time">出校时间：{formatted()}</p>
            <div class="name_area">
                <p>{data.name}</p>
                <p>{data.gender}</p>
                <p>{data.id}</p>
            </div>
        </div>
    </div>
{/if}

<style>
    .bg {
        position: absolute;
        width: 100%;
        height: 100%;
        max-width: 600px;
        max-height: 1200px;
        background-color: #8ad1fd;
    }
    p {
        font-family: Arial, Helvetica, sans-serif;
        line-height: 100%;
    }
    .class {
        position: relative;
        top: 28%;
        width: 100%;
        text-align: center;
        font-size: 20px;
    }
    .leave_time {
        position: relative;
        top: 28%;
        width: 100%;
        text-align: center;
        font-size: 18px;
        color: #9d9d9d;
    }
    .name_area {
        position: relative;
        top: 56.5%;
        width: 68%;
        margin: auto;
        display: flex;
        justify-content: space-between;
        align-items: center;
    }
    .leave {
        background: url("/leave.jpg");
        background-size: 100% auto;
        background-repeat: no-repeat;
    }
    .back {
        background: url("/back.jpg");
        background-size: 100% auto;
        background-repeat: no-repeat;
    }
</style>
