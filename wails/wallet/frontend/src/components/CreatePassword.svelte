<script lang="ts">
  export let handleClick: () => void = () => {};
  export let walletLabel: string = '';
  let passwordRegex: RegExp = /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d).{12,}$/;
  function isPasswordValid(password): boolean {
    return passwordRegex.test(password);
  }
  
  function checkPasswordStrength(): void {
    const passwordInput = document.getElementById('wallet-password') as HTMLInputElement;
    const passwordStrengthContainer = document.getElementById('password-strength') as HTMLParagraphElement;
    if (!passwordInput || !passwordStrengthContainer) {
      console.error('Password input or strength container not found');
      return;
    }

    const password: string = passwordInput.value;
    const isPasswordStrong: boolean = isPasswordValid(password);
    passwordStrengthContainer.style.display = 'block';
    if (isPasswordStrong) {
      passwordStrengthContainer.textContent = 'Password strength: Strong';
    } else if (password.length > 0 &&  !isPasswordStrong) {
      passwordStrengthContainer.textContent = 'Password strength: Weak';
    } else {
      passwordStrengthContainer.textContent = '';
      passwordStrengthContainer.style.display = 'none';
    }
  }
  
  function checkPasswordsMatch(): void {
    const passwordInput = document.getElementById('wallet-password') as HTMLInputElement;
    const confirmPasswordInput = document.getElementById('confirm-password') as HTMLInputElement;
    const passwordMatchContainer = document.getElementById('password-match') as HTMLParagraphElement;
    const createPasswordButton = document.getElementById('create-password-button') as HTMLButtonElement;
    if (!passwordInput || !confirmPasswordInput || !passwordMatchContainer || !createPasswordButton) {
      console.error('Password inputs or match container not found');
      return;
    }
    
    const password: string = passwordInput.value;
    const isPasswordStrong = isPasswordValid(password);
    const confirmPassword: string = confirmPasswordInput.value;
    if (password !== confirmPassword) {
      passwordMatchContainer.style.display = 'block';
      passwordMatchContainer.textContent = 'Passwords do not match';
    } else {
        passwordMatchContainer.textContent = '';
        passwordMatchContainer.style.display = 'none';
      }
    
    if (isPasswordStrong && password === confirmPassword) {
      createPasswordButton.disabled = false;
      createPasswordButton.style.backgroundColor = 'black';
    }
  }   
</script>
    
<div class="header-content">
  <h2>Create Password</h2>
  <p>This password will unlock your wallet on this application.</p>
</div>
<form class="form-container">
  <div class="input-group">
    <label for="password">New password (16 characters min, at least one character should be in lowercase, one in uppercase and one number)</label>
    <input type="password" id="wallet-password" on:input={checkPasswordStrength} />
    <p id="password-strength" class="validation-label" ></p>
  </div>
  <div class="input-group">
    <label for="confirm-password">Confirm password</label>
    <input type="password" id="confirm-password" on:input={checkPasswordsMatch} />
    <p id="password-match" class="validation-label" ></p>
  </div>
  <div class="create-password-button-container">
  <button id="create-password-button" type="button" disabled=true on:click={handleClick}>{walletLabel}</button>
</div>
</form>
  
<style>
.header-content {
  text-align: center;
  margin-bottom: 10px;
}

h2 {
  font-size: 2rem;
  color: #0066cc;
  margin-bottom: 5px;
}

p {
  font-size: 1rem;
  color: #555;
  margin-bottom: 20px;
}

.form-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  align-content: center;
  gap: 2px;
  width: 100%;
  max-width: 400px;
  background-color: #fff;
  padding: 5px;
  margin-top: 0px;
  border-radius: 10px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
}

.input-group {
  display: flex;
  flex-direction: column;
  gap: 15px;
  align-items: center;
}

label {
  font-size: 1rem;
  font-weight: bold;
  color: #222;
}

input {
  padding: 4px;
  font-size: 1rem;
  border: 1px solid #ccc;
  border-radius: 5px;
  max-width: 190px;
  color: #333;
  background-color: #fefefe;
}

input:focus {
  outline: none;
  border-color: #0066cc;
  box-shadow: 0 0 3px #0066cc;
}

.create-password-button-container {
  display: flex;
  justify-content: center;
  width: 100%;
}

#create-password-button {
  text-align: center;
  color: white;
  background-color: #0066cc;
  font-family: "Nunito", sans-serif;
  font-weight: bold;
  width: 60%;
  margin-top:15px;
  height: 25px;
  border-radius: 25px;
  cursor: pointer;
  border: none;
  transition: background-color 0.3s ease;
}

#create-password-button:hover:not(:disabled) {
  background-color: #004c99;
}

#create-password-button:disabled {
  background-color: #ccc;
  color: #666;
  cursor: not-allowed;
}

.validation-label {
  font-size: 0.9rem;
  color: red;
  display: none;
}

@media (max-width: 768px) {
  h2 {
    font-size: 1.8rem;
  }

  .form-container {
    padding: 15px;
  }

  #create-password-button {
    height: 40px;
  }
}

@media (max-width: 480px) {
  h2 {
    font-size: 1.5rem;
  }

  label,
  input,
  #create-password-button {
    font-size: 0.9rem;
  }

  .form-container {
    gap: 15px;
  }
}
</style>
    