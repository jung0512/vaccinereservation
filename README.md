# vaccinereservation
백신예약 페이지
# header
``` html
<header id="header">
	<h2>(과정평가형 정보처리산업기사) 백신예약 프로그램 ver 202109</h2>
</header>
```
# nav
``` html
<nav id="nav">
	<ul>
		<li><a href="join.jsp">백신예약</a>
		<li><a href="member_list.jsp">백신예약리스트</a>
		<li><a href="select_reservation_table.jsp">백신예약조회</a>
		<li><a href="reservation_status.jsp">백신예약현황</a>
		<li><a href="index.jsp">홈으로</a>
	</ul>
</nav>
```
#section
``` html
<section class="section">
	<h2>백신접종 시 주의사항</h2>
	<p>
	내용
	</p>
</section>
```
# footer
``` html
<footer id="footer">
	<p>HRDKOREA Copyright&copy;2015 All rights reserved. Human Resources Development Service of Korea.</p>
</footer>
```
# join
쿼리문 
``` db
<%
	String sql = "SELECT MAX(RESVNO) FROM tbl_vaccresv_202108";
	
	Connection conn = DBConnect.getConnection();
	PreparedStatement pstmt = conn.prepareStatement(sql);
	ResultSet rs = pstmt.executeQuery();
	
	rs.next();
	int num = rs.getInt(1)+1;
%>
```
자바스크립트
``` script
<script>
	function checkValue(){
		if(!document.data.jumin.value){
			alert("주민번호를 입력하세요.");
			data.jumin.focus();
			return false;
		}
		else if(!document.data.vcode.value){
			alert("백신코드를 입력하세요.");
			data.vcode.focus();
			return false;
		}
		else if(!document.data.hospcode.value){
			alert("병원코드를 입력하세요.");
			data.hospcode.focus();
			return false;
		}
		else if(!document.data.resvdate.value){
			alert("예약번호를 입력하세요.");
			data.resvdate.focus();
			return false;
		}
		else if(!document.data.resvtime.value){
			alert("예약시간를 입력하세요.");
			data.resvtime.focus();
			return false;
		}
		return true;
	}
</script>
```
html
``` html
<header>
		<jsp:include page="layout/header.jsp"></jsp:include>
	</header>
	
	<nav>
		<jsp:include page="layout/nav.jsp"></jsp:include>
	</nav>
	
	<section class="section">
		<h2>백신예약</h2><br>
		<form name="data" action="join_p.jsp" method="post" onsubmit="return checkValue()">
			<table class="table_line">
				<tr>
					<th>예약번호</th>
					<td><input type="text" name="resvno" value="<%= num %>" readonly>예)20210011</td>
				</tr>
				<tr>
					<th>주민번호</th>
					<td><input type="text" name="jumin">예)790101-1111111</td>
				</tr>
				<tr>
					<th>백신코드</th>
					<td><input type="text" name="vcode">예)V001, V002, V003</td>
				</tr>
				<tr>
					<th>병원코드</th>
					<td><input type="text" name="hospcode">예)H001, H002, H003, H004</td>
				</tr>
				<tr>
					<th>예약날짜</th>
					<td><input type="text" name="resvdate">예)20210101</td>
				</tr>
				<tr>
					<th>에약시간</th>
					<td><input type="text" name="resvtime">예)0930, 1130</td>
				</tr>
				<tr class="center">
					<td colspan="2">
					<input type="submit" value="등록">
					<input type="button" value="취소"></td>
				</tr>
			</table>
		</form>
	</section>
	
	<footer>
		<jsp:include page="layout/footer.jsp"></jsp:include>
	</footer>
```
# join_p
``` db
<%
	String sql="insert into tbl_vaccresv_202108 values (?, ?, ?, ?, ?, ?)";
	
	Connection conn = DBConnect.getConnection();
	PreparedStatement pstmt = conn.prepareStatement(sql);
	
	pstmt.setInt(1, Integer.parseInt(request.getParameter("resvno")));
	pstmt.setString(2, request.getParameter("jumin"));
	pstmt.setString(3, request.getParameter("hospcode"));
	pstmt.setString(4, request.getParameter("resvdate"));
	pstmt.setString(5, request.getParameter("resvtime"));
	pstmt.setString(6, request.getParameter("vcode"));
	
	pstmt.executeUpdate();
%>
```
