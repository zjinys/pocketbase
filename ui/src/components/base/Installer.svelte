<script>
    import { createEventDispatcher } from "svelte";
    import ApiClient from "@/utils/ApiClient";
    import Field from "@/components/base/Field.svelte";

    const dispatch = createEventDispatcher();

    let email = "";
    let password = "";
    let passwordConfirm = "";
    let isLoading = false;

    async function submit() {
        if (isLoading) {
            return;
        }

        isLoading = true;

        try {
            await ApiClient.admins.create({
                email,
                password,
                passwordConfirm,
            });

            await ApiClient.admins.authWithPassword(email, password);

            dispatch("submit");
        } catch (err) {
            ApiClient.errorResponseHandler(err);
        }

        isLoading = false;
    }
</script>

<form class="block" autocomplete="off" on:submit|preventDefault={submit}>
    <div class="content txt-center m-b-base">
        <h4>创建管理员</h4>
    </div>

    <Field class="form-field required" name="email" let:uniqueId>
        <label for={uniqueId}>邮件地址</label>
        <!-- svelte-ignore a11y-autofocus -->
        <input type="email" autocomplete="off" id={uniqueId} bind:value={email} required autofocus />
    </Field>

    <Field class="form-field required" name="password" let:uniqueId>
        <label for={uniqueId}>密码</label>
        <input
            type="password"
            autocomplete="new-password"
            minlength="10"
            id={uniqueId}
            bind:value={password}
            required
        />
        <div class="help-block">最少10个字符.</div>
    </Field>

    <Field class="form-field required" name="passwordConfirm" let:uniqueId>
        <label for={uniqueId}>重复密码</label>
        <input type="password" minlength="10" id={uniqueId} bind:value={passwordConfirm} required />
    </Field>

    <button
        type="submit"
        class="btn btn-lg btn-block btn-next"
        class:btn-disabled={isLoading}
        class:btn-loading={isLoading}
    >
        <span class="txt">创建并登录</span>
        <i class="ri-arrow-right-line" />
    </button>
</form>
