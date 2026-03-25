<script>
  let formData = {
    email: '',
    password: '',
    target_language: 'en',
    native_language: 'zh-CN',
    timezone: 'UTC',
    level_self_assign: 1
  };

  let message = '';
  let error = '';
  let isLoading = false;

  const BASE_URL = 'https://api.libiamo.uk/v1';

  async function handleRegister() {
    message = '';
    error = '';
    isLoading = true;

    try {
      const response = await fetch(`${BASE_URL}/auth/register`, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        credentials: 'include', 
        body: JSON.stringify(formData)
      });

      const data = await response.json();

      if (response.ok) {
        message = `注册成功！ID: ${data.id}`;
      } else {
        error = data.message || data.err || '注册失败';
      }
    } catch (e) {
      error = "网络错误";
    } finally {
      isLoading = false;
    }
  }
</script>

<div class="container">
  <h2>用户注册</h2>
  
  <form on:submit|preventDefault={handleRegister}>
    <label for="reg-email">邮箱:</label>
    <input id="reg-email" type="email" bind:value={formData.email} required />

    <label for="reg-password">密码:</label>
    <input id="reg-password" type="password" bind:value={formData.password} required />

    <label for="reg-target">目标语言:</label>
    <select id="reg-target" bind:value={formData.target_language}>
      <option value="en">英语 (en)</option>
      <option value="es">西班牙语 (es)</option>
      <option value="fr">法语 (fr)</option>
    </select>

    <label for="reg-native">母语:</label>
    <input id="reg-native" type="text" bind:value={formData.native_language} required />

    <label for="reg-timezone">时区:</label>
    <input id="reg-timezone" type="text" bind:value={formData.timezone} />

    <label for="reg-level">自我评估水平 (1-5):</label>
    <input id="reg-level" type="number" min="1" max="5" bind:value={formData.level_self_assign} />

    {#if error}
      <div class="error">{error}</div>
    {/if}
    
    {#if message}
      <div class="success">{message}</div>
    {/if}

    <button type="submit" disabled={isLoading}>
      {isLoading ? '提交中...' : '提交注册'}
    </button>
  </form>
</div>

<style>
  .container { max-width: 400px; margin: 2rem auto; font-family: sans-serif; padding: 1rem; border: 1px solid #ddd; border-radius: 8px; }
  label { display: block; margin-top: 1rem; font-weight: bold; font-size: 0.9rem; }
  input, select { width: 100%; padding: 8px; margin-top: 4px; box-sizing: border-box; }
  button { width: 100%; margin-top: 1.5rem; padding: 10px; background: #007bff; color: white; border: none; border-radius: 4px; cursor: pointer; }
  .error { color: red; background: #fee; padding: 10px; margin-top: 10px; border-radius: 4px; }
  .success { color: green; background: #efe; padding: 10px; margin-top: 10px; border-radius: 4px; }
</style>
