<script>
	// JS here
  const strengthText = ["", "ruim 💩", "ok 😐", "melhorou 🙂", "brabo apenas💪"];

  let strength = 0;
  let validations = [];
  let showPassword = false;

  function validatePassword(event) {
    const password = event.target.value;

    validations = [
      (password.length > 5),
      (password.search(/[A-Z]/) > -1),
      (password.search(/[0-9]/) > -1),
      (password.search(/[$&+,:;=?@#]/) > -1),
    ]

    strength = validations.reduce((accumulatorValue, currentValue) =>
      accumulatorValue + currentValue
    )
  }

</script>

<style>
	/* CSS here */
  form {
    --text-color: #afafaf;
    max-width: 500px;
  }

  ul {
    list-style: none;
  }

  .field {
    width: 100%;
    position: relative;
    border-bottom: 2px dashed var(--text-color);
    margin: 4rem auto 1rem;
    transition: 500ms;
  }

  .label {
    color: var(--text-color);
    font-size: 1.2rem;
  }

  .input {
    outline: none;
    border: none;
    overflow: hidden;
    margin: 0;
    width: 100%;
    padding: 0.25rem 0;
    background: none;
    color: white;
    font-size: 1.2rem;
    font-weight: bold;
  }

  .input:valid {
    color: yellowgreen;
  }

  .input:invalid {
    color: orangered;
  }

  /* border animation  */

  .field::after {
    content: "";
    position: relative;
    display: block;
    height: 4px;
    width: 100%;
    background: #d16dff;
    transform: scaleX(0);
    transform-origin: 0%;
    transition: transform 500ms ease;
    top: 2px;
  }

  .field:focus-within {
    border-color: transparent;
  }

  .field:focus-within::after {
    transform: scaleX(1);
  }

  /* label animation */

  .label {
    z-index: -1;
    position: absolute;
    transform: translateY(-2rem);
    transform-origin: 0%;
    transition: transform 400ms;
  }

  .field:focus-within .label,
  .input:not(:placeholder-shown) + .label {
    transform: scale(0.8) translateY(-5rem);
    opacity: 1;
  }

  /* strength meter */

  .strength {
    display: flex;
    height: 20px;
    width: 100%;
  }

  .bar {
    margin-right: 5px;
    height: 100%;
    width: 25%;
    transition: box-shadow 500ms;
    box-shadow: inset 0px 20px #1f1f1f;
  }

  .bar-show {
    box-shadow: none;
  }

  .bar-1 {
    background: linear-gradient(to right, red, orangered);
  }

  .bar-2 {
    background: linear-gradient(to right, orangered, yellow);
  }

  .bar-3 {
    background: linear-gradient(to right, yellow, yellowgreen);
  }

  .bar-4 {
    background: linear-gradient(to right, yellowgreen, green);
  }
  .strength-text {
    margin-top: 20px;
    font-weight: bold;
  }

  /* buttons styles */

  button {
    margin-top: 2rem;
    padding: 10px 30px;
    font-weight: bold;
    border: 2px solid greenyellow;
    color: greenyellow;
    border-radius: 100px;
    background: transparent;
    transition: all 1000ms;
  }

  button:disabled {
    border-color: var(--text-color);
    color: var(--text-color);
  }
  .toggle-password {
    position: absolute;
    cursor: help;
    font-size: 0.8rem;
    right: 0.25rem;
    bottom: 0.5rem;
  }

</style>

<!-- HTML here -->

<main>
	<form autocomplete="off">
		<div class="field">
      <input
        type="email"
        name="email"
        class="input"
        placeholder=" "
      />
      <label for="email"  class="label">Email</label>
		</div>

		<div class="field">
      <input
        type={showPassword ? 'text' : 'password'}
        class="input"
        placeholder=" "
        maxlength="10"
        on:input={validatePassword}
      />
      <label for="password" class="label">Password</label>

      <span
        class="toggle-password"
        on:mouseenter={() => (showPassword = true)}
        on:mouseleave={() => (showPassword = false)}
      >
        {showPassword ? '🙈' : '👀'}
      </span>
		</div>

    <div class="strength">
      <span class="bar bar-1" class:bar-show={strength > 0}></span>
      <span class="bar bar-2" class:bar-show={strength > 1}></span>
      <span class="bar bar-3" class:bar-show={strength > 2}></span>
      <span class="bar bar-4" class:bar-show={strength > 3}></span>
    </div>

    <div class="strength-text">{strengthText[strength]}</div>

    {#if validations.length}
      <ul>
        <li> {validations[0] ? '✅' : '❌'}
          &nbsp precisa ter no mínimo 5 caracteres
        </li>
        <li> {validations[1] ? '✅' : '❌'}
          &nbsp precisa conter uma letra maiúscula
        </li>
        <li> {validations[2] ? '✅' : '❌'}
          &nbsp precisa conter um número
        </li>
        <li> {validations[3] ? '✅' : '❌'}
          &nbsp precisa conter um dos $&+,:;=?@#
        </li>
      </ul>
    {/if}

    <button disabled={strength < 4}>Logar</button>
	</form>
</main>
