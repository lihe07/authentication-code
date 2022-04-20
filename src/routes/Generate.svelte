<script>
    import pako from "pako";
    import qrcode from "qrcode";
    function encode(data) {
        return window.btoa(pako.deflate(JSON.stringify(data)));
    }

    // console.log(encode({ msg: "Hello World!" }));

    // let encoded = encode({ msg: "Hello World!" });
    // console.log(decode(encoded));

    let data = {
        class: undefined,
        name: undefined,
        gender: undefined,
        id: undefined,
        type: undefined,
    };

    function submit() {
        if (data.class === undefined) {
            alert("请输入班级名称");
            return;
        }
        if (data.class.length < 2) {
            alert("班级名称不能少于2个字符");
            return;
        }
        if (data.name === undefined) {
            alert("请输入姓名");
            return;
        }
        if (data.name.length < 2) {
            alert("姓名不能少于2个字符");
            return;
        }
        if (data.id === undefined) {
            alert("请输入学号");
            return;
        }
        if (data.id === null || Number.isNaN(data.id)) {
            alert("学号必须是数字");
            return;
        }
        qrcode.toCanvas(
            code_canvas,
            location.origin + "/#/display/" + encode(data),
            (err) => {
                if (err) {
                    console.error(err);
                    alert("生成二维码失败");
                }
                alert("生成成功");
            }
        );
    }

    let code_canvas;
</script>

<main>
    <h2>生成新的二维码</h2>

    <label for="class">班级名称:</label>
    <input id="class" type="text" bind:value={data.class} />
    <br />
    <label for="name">姓名:</label>
    <input id="name" type="text" bind:value={data.name} />
    <br />
    <label for="gender">性别:</label>
    <select id="gender" bind:value={data.gender}>
        <option value="男">男</option>
        <option value="女">女</option>
    </select>
    <br />
    <label for="id">学号:</label>
    <input id="id" type="number" bind:value={data.id} />
    <br />
    <label for="type">类型:</label>
    <select id="type" bind:value={data.type}>
        <option value="leave">离校</option>
        <option value="back">返校</option>
    </select>

    <br />
    <button on:click={submit}>提交</button>
    <br />
    <canvas bind:this={code_canvas} />
</main>

<style>
    main {
        max-width: 50rem;
        margin: 20px auto;
    }
    br {
        margin: 10px;
    }
</style>
