<template>
    <div class="card">
      <div class="card-header">
        <slot name="header">
          <h2>Заголовок по умолчанию</h2>
        </slot>
      </div>
  
      <div class="card-body">
        <slot>
          <p>Основное содержимое по умолчанию.</p>
        </slot>
      </div>
  
      <div class="card-footer">
        <slot name="footer">
          <p>Футер по умолчанию</p>
        </slot>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: 'UserCard',
  };
  </script>