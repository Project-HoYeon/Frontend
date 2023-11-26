<script>
  let user = {
    name: "홍길동",
    email: "honggildong@example.com",
    department: "컴퓨터공학과",
    studentId: "20230001",
  };

</script>
<div class="profile-container">

  <img src="assets/logo.svg" alt="profile">

  <div class="profile-info">
    <h2>{user.name}</h2>
    <p>{user.email}</p>
    <p>{user.department}</p>
    <p>{user.studentId}</p>
  </div>
</div>
<style>
  .profile-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 1.25rem;
    border: 1px solid #ccc;
    border-radius: 0.625rem;
    max-width: 25rem;
    margin: auto;
  }

  .profile-image {
    width: 6.25rem;
    height: 6.25rem;
    border-radius: 50%;
    background-size: cover;
    margin-bottom: 1.25rem;
  }

  .profile-info {
    text-align: center;
  }

  .profile-info h2 {
    margin: 0;
    color: #333;
    font-size: 1.5rem;
  }

  .profile-info p {
    margin: 5px 0;
    color: #666;
    font-size: 1rem;
  }

  img {
    height: 4rem;
    width: 4rem;
    border-radius: 9px;
    margin-bottom : 1rem;
  }
</style>
