<script>
	import '../app.css';
	// import { Link, Route } from "svelte-routing";
</script>


<header class="bg-blue-500 py-4 text-white">
	<div class="container mx-auto">
	  <h1 class="text-2xl font-bold">Easy Calc</h1>
	</div>
  </header>
  
  <main class="container mx-auto my-8">
	<slot></slot>
  </main>
  
  <footer class="bg-blue-500 py-4 text-white">
	<div class="container mx-auto">
	  <p>&copy; 2023 Your Company</p>
	</div>
  </footer>
  
  <style>
	/* 추가적인 스타일링을 여기에 추가할 수 있습니다. */
  </style>