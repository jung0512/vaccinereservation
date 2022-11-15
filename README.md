# vaccinereservation
백신예약 페이지
# header
``` html
<header id="header">
	<h2>(과정평가형 정보처리산업기사) 백신예약 프로그램 ver 202109</h2>
</header>
```
![image](https://user-images.githubusercontent.com/102035198/201814228-fe2379ed-e04a-44e4-a01c-5a8b807ea83b.png)<br>
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
![image](https://user-images.githubusercontent.com/102035198/201814259-949d9aea-ce01-4b24-9e65-8a10061e3520.png)<br>
#section
``` html
<section class="section">
	<h2>백신접종 시 주의사항</h2>
	<p>
	내용
	</p>
</section>
```
![image](https://user-images.githubusercontent.com/102035198/201814297-d419ae10-c9dd-40c7-8fd1-54ca35c50a8d.png)<br>
# footer
``` html
<footer id="footer">
	<p>HRDKOREA Copyright&copy;2015 All rights reserved. Human Resources Development Service of Korea.</p>
</footer>
```
![image](https://user-images.githubusercontent.com/102035198/201814317-a4279d8e-676c-46af-8797-fa07b0cb3721.png)<br>
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
예약번호의 마지막 번호를 가져와 준다<br>
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
입력되지 않을 칸이 있을경우 경고창을 띄운 다음 그 칸으로 
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
![image](https://user-images.githubusercontent.com/102035198/201814396-06b50661-c821-4c6d-9ced-79500f016232.png)<br>
# join_p
```
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
입력한 정보를 테이블에 
# search
``` script
<script type="text/javascript">
	function checkValue2() {
		if(!document.sData.rNum.value) {
			alert("회원번호를 입력하세요.");
			sData.rNum.focus();
			return false;
		} 
		return true;
		}
</script>
```
입력하였는지 확인하여 입력되지 않았을 시 경고창을 띄운 다음 그칸으로 이동한다<br>
``` html
<header>
	  <jsp:include page="layout/header.jsp"></jsp:include>
 </header>

 <nav>
   	 <jsp:include page="layout/nav.jsp"></jsp:include>
 </nav>
		
 <section class="section">
		<h2>백신예약조회</h2>
		<form name="sData" action="search_reservation_table.jsp" method="post" onsubmit="return checkValue2()">
			<table class="table_line">
				<tr>
					<th>예약번호</th>
					<td><input type="text" name="rNum"></td>
				</tr>
				<tr>
					<th colspan="2">
						<input type="submit" value="조회하기">
						<input type="button" value="홈으로" onclick="location.href='index.jsp'">
					</th>
				</tr>
			</table>
		</form>
 </section>	
 <footer>
	<jsp:include page="layout/footer.jsp"></jsp:include>
 </footer>
```
조회하기를 누르면 조회 결과가 나온다
# search_table
``` 쿼리문
<%
	int rNo = Integer.parseInt(request.getParameter("rNum"));

	StringBuffer sb = new StringBuffer();

	sb.append(" select v.RESVNO                                                                 ")
	.append(" ,j.NAME                                                                       ")
	.append(" ,case substr(v.jumin, 8, 1)                                                   ")
	.append(" 	when '1' then '남'                                                          ")
	.append(" 	when '2' then '여'                                                          ")
	.append(" end as gender                                                                 ")
	.append(" ,case v.HOSPCODE                                                              ")
	.append(" 	when 'H001' then '가_병원'                                                    ")
	.append(" 	when 'H002' then '나_병원'                                                    ")
	.append(" 	when 'H003' then '다_병원'                                                    ")
	.append(" 	when 'H004' then '라_병원'                                                    ")
	.append(" end as hospcode                                                               ")
	.append(" ,to_char(v.RESVDATE,'yyyy\"년\"mm\"월\"dd\"일\"') as  RESVDATE                  ")
	.append(" ,substr(to_char(v.RESVTIME, 'FM0000'),1,2)                                      ")
	.append(" 	|| ':' || substr(to_char(v.RESVTIME, 'FM0000'),3,2) as RESVTIME               ")
	.append(" ,case VCODE                                                                     ")
	.append(" 	when 'V001' then 'A백신'                                                      ")
	.append(" 	when 'V002' then 'B백신'                                                      ")
	.append(" 	when 'V003' then 'C백신'                                                      ")
	.append(" end as vcode                                                                    ")
	.append(" ,case h.HOSPADDR                                                                ")
	.append(" 	when '10' then '서울'                                                         ")
	.append(" 	when '20' then '대전'                                                         ")
	.append(" 	when '30' then '대구'                                                         ")
	.append(" 	when '40' then '광주'                                                         ")
	.append(" end as HOSPADDR                                                                 ")
	.append(" from TBL_VACCRESV_202108 v, TBL_JUMIN_202108 j, TBL_HOSP_202108 h               ")
	.append(" where v.JUMIN = j.JUMIN and v.hospcode = h.hospcode                             ")
	.append(" and v.RESVNO =").append(rNo);


	String sql = sb.toString();
    
    Connection conn = DBConnect.getConnection();
    PreparedStatement ps = conn.prepareStatement(sql);
	ResultSet rs = ps.executeQuery();   
    
%>
```
표시형식을 
``` html
<header>
	  <jsp:include page="layout/header.jsp"></jsp:include>
</header>

<nav>
   	 <jsp:include page="layout/nav.jsp"></jsp:include>
</nav>
		
<section>
	
		<h2>예약번호 <%=rNo %>님의 예약조회</h2>
		 <%if(rs.next()){ %>
			<table class="table_line">
				<tr>
					<th>예약번호</th>
					<th>성명</th>
					<th>성별</th>
					<th>병원이름</th>
					<th>예약날짜</th>
					<th>예약시간</th>
					<th>백신코드</th>
					<th>병원지역</th>
				</tr>
				<tr>
					<td><%=rs.getString(1) %></td>
					<td><%=rs.getString(2) %></td>
					<td><%=rs.getString(3) %></td>
					<td><%=rs.getString(4) %></td>
					<td><%=rs.getString(5) %></td>
					<td><%=rs.getString(6) %></td>
					<td><%=rs.getString(7) %></td>
					<td><%=rs.getString(8) %></td>
				</tr>
			</table>
			<%}else{ %>
				<p align="center">회원번호 <%= rNo %>의 회원 정보가 없습니다.</p>
				
						
		   <% } %>
				<input type="button" value="돌아가기" onclick="location.href='search.jsp'">
</section>
<footer>
	<jsp:include page="layout/footer.jsp"></jsp:include>
</footer>
```
조회 결과를 표시해
![image](https://user-images.githubusercontent.com/102035198/201834409-1d7ef79e-4afe-429f-a6cd-5d7d561c05a9.png)<br>
![image](https://user-images.githubusercontent.com/102035198/201835049-43851926-8b29-4e8d-93f2-c208b87b148d.png)<br>
