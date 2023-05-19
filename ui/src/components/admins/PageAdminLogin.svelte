<script>
    import { link, replace, querystring } from "svelte-spa-router";
    import FullPage from "@/components/base/FullPage.svelte";
    import ApiClient from "@/utils/ApiClient";
    import Field from "@/components/base/Field.svelte";
    import { addErrorToast, removeAllToasts } from "@/stores/toasts";

    const queryParams = new URLSearchParams($querystring);

    let email = queryParams.get("demoEmail") || "";
    let password = queryParams.get("demoPassword") || "";
    let isLoading = false;

    function login() {
        if (isLoading) {
            return;
        }

        isLoading = true;

        return ApiClient.admins
            .authWithPassword(email, password)
            .then(() => {
                removeAllToasts();
                replace("/");
            })
            .catch(() => {
                addErrorToast("Invalid login credentials.");
            })
            .finally(() => {
                isLoading = false;
            });
    }
</script>

<FullPage>
    <form class="block" on:submit|preventDefault={login}>
        <div class="content txt-center m-b-base">
            <h4>系统登录</h4>
        </div>

        <Field class="form-field required" name="identity" let:uniqueId>
            <label for={uniqueId}>邮件地址</label>
            <!-- svelte-ignore a11y-autofocus -->
            <input type="email" id={uniqueId} bind:value={email} required autofocus />
        </Field>

        <Field class="form-field required" name="password" let:uniqueId>
            <label for={uniqueId}>密码</label>
            <input type="password" id={uniqueId} bind:value={password} required />
<!--            <div class="help-block">-->
<!--                <a href="/request-password-reset" class="link-hint" use:link>Forgotten password?</a>-->
<!--            </div>-->
        </Field>

        <button
            type="submit"
            class="btn btn-lg btn-block btn-next"
            class:btn-disabled={isLoading}
            class:btn-loading={isLoading}
        >
            <span class="txt">登录</span>
            <i class="ri-arrow-right-line" />
        </button>
    </form>
</FullPage>
