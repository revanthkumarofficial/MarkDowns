# JSP

##### 

```
   <!-- SPECIFY LANGUAGE, CONTENT TYPE AND ENCODING -->
   
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
  
   <!-- ENABLING EXPRESSION LANGUAGE -->
   
<%@ page isELIgnored="false" %>

<!-- PAGE SESSION -->

<%@ page session="false" %>


<!-- IMPORT JAVA CLASSES -->

<%@page import="com.mars.cdma.gov.utils.CommonUtils"%>


```

### LIBRARIES

1. JAVA

```
 <!-- JSTL CORE  -->
 
<%@ taglib uri="https://java.sun.com/jsp/jstl/core" prefix="c" %>

<!-- JSTL FORMAT -->
<%@ taglib prefix="fmt" uri="https://java.sun.com/jsp/jstl/fmt" %>

```

2. SPRING


```
<!-- SPRING CORE  -->

<%@ taglib prefix="spring" uri="http://www.springframework.org/tags"%>


<!-- SPRING FORM  -->

<%@ taglib uri="https://www.springframework.org/tags/form" prefix="springForm"%>

```



##### GEETING URLS

```
<!-- Getting Path -->
<%
	String path = request.getContextPath();
%>

<!-- Getting URL -->
<script type="text/javascript">
	$(function() {

		var url = "${pageContext.request.contextPath}/totalVisitCount.do";

		$.ajax({

		url
	});
</script>
```



##### SPRING MVC FORM
1. LOGIN
```
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "https://www.w3.org/TR/html4/loose.dtd">
<%@ taglib uri="https://www.springframework.org/tags/form"
	prefix="springForm"%>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Customer Save Page</title>
<style>
.error {
	color: #ff0000;
	font-style: italic;
	font-weight: bold;
}
</style>
</head>
<body>

	<springForm:form method="POST" commandName="customer"
		action="save.do">
		<table>
			<tr>
				<td>Name:</td>
				<td><springForm:input path="name" /></td>
				<td><springForm:errors path="name" cssClass="error" /></td>
			</tr>
			<tr>
				<td>Email:</td>
				<td><springForm:input path="email" /></td>
				<td><springForm:errors path="email" cssClass="error" /></td>
			</tr>
			<tr>
				<td>Age:</td>
				<td><springForm:input path="age" /></td>
				<td><springForm:errors path="age" cssClass="error" /></td>
			</tr>
			<tr>
				<td>Gender:</td>
				<td><springForm:select path="gender">
						<springForm:option value="" label="Select Gender" />
						<springForm:option value="MALE" label="Male" />
						<springForm:option value="FEMALE" label="Female" />
					</springForm:select></td>
				<td><springForm:errors path="gender" cssClass="error" /></td>
			</tr>
			<tr>
				<td>Birthday:</td>
				<td><springForm:input path="birthday" placeholder="MM/dd/yyyy"/></td>
				<td><springForm:errors path="birthday" cssClass="error" /></td>
			</tr>
			<tr>
				<td>Phone:</td>
				<td><springForm:input path="phone" /></td>
				<td><springForm:errors path="phone" cssClass="error" /></td>
			</tr>
			<tr>
				<td colspan="3"><input type="submit" value="Save Customer"></td>
			</tr>
		</table>

	</springForm:form>

</body>
</html>
```

2. 

```

<%@ taglib uri="https://java.sun.com/jsp/jstl/core" prefix="c" %>
<%@ taglib prefix="fmt" uri="https://java.sun.com/jsp/jstl/fmt" %>

<%@ page session="false" %>
<html>
<head>
	<title>Customer Saved Successfully</title>
</head>
<body>
<h3>
	Customer Saved Successfully.
</h3>

<strong>Customer Name:${customer.name}</strong><br>
<strong>Customer Email:${customer.email}</strong><br>
<strong>Customer Age:${customer.age}</strong><br>
<strong>Customer Gender:${customer.gender}</strong><br>
<strong>Customer Birthday:<fmt:formatDate value="${customer.birthday}" type="date" /></strong><br>

</body>
</html>
```










##### SAMPLE JSPS

1. advertisementHoarding
```
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
<title>Non Hoarding Advertisement</title>
<%
	String path = request.getContextPath();
%>
</head>
<%@ taglib uri="/WEB-INF/tlds/tiles-jsp.tld" prefix="tiles"%>
<%@ taglib uri="/WEB-INF/tlds/fmt.tld" prefix="fmt"%>
<%@ taglib uri="/WEB-INF/tlds/c.tld" prefix="c"%>
<%@ taglib uri="/WEB-INF/tlds/spring.tld" prefix="spring"%>
<%@ taglib uri="/WEB-INF/tlds/spring-form.tld" prefix="form"%>
<%@ taglib uri="/WEB-INF/tlds/fn.tld" prefix="fn"%>

<script src="<%=path%>/jsp/js/jquery.min.js"></script>
<script src="<%=path%>/jsp/js/bootstrap.min.js"></script>
<link rel="stylesheet" href="<%=path%>/jsp/css/bootstrap.min.css">
<link rel="stylesheet" href="<%=path%>/jsp/css/bootstrap.3.3.7.min.css">

<script type="text/javascript">
	$(document).ready(function() {

		var fromDate = $("#startDate").datepicker({
			defaultDate : "+0w",
			dateFormat : 'mm/dd/yy',
			changeMonth : true,
			//numberOfMonths: 2,
			minDate : 0,

			onSelect : function(date) {
				var date2 = $('#startDate').datepicker('getDate');
				var datemin = $('#startDate').datepicker('getDate');
				date2.setMonth(date2.getMonth() + 11);
				$('#endDate').datepicker('option', 'minDate', datemin);
			}
		});
		var toDate = $("#endDate").datepicker({
			defaultDate : "+1w",
			minDate : 0,
			dateFormat : 'mm/dd/yy',
			maxDate : new Date(2019, 02, 31),
			changeMonth : true,
			changeYear : true,

		});

		//For Read-Only Fromdate
		$("#startDate").click(function() {
			$(this).blur();
		});
		$("#startDate").keyup(function() {
			$(this).blur();
		});

		//For Read-Only Todate 
		$("#endDate").click(function() {
			$(this).blur();
		});
		$("#endDate").keyup(function() {
			$(this).blur();
		});

	});
</script>

<form action="advertiseHoarding.do" method="POST" name="advAppForm"
	enctype='multipart/form-data' id="advAppForm"
	onsubmit="return checkValidation();">
	<fieldset>

		<div class="panel panel-primary">
			<div class="panel-heading">Application for Hoarding
				Advertisement</div>
			<div class="panel-body">
				<div class="row">
					<div class="col-md-12">
						<div class="col-md-6">
							<div class="form-group">
								<label class="col-md-6 control-label" for="textinput">Sur-Name
									of Advertiser/Applicant</label>
								<div class="col-md-6" style="margin-bottom: 15px;">
									<input class="form-control" name="applicantSurName"
										id="applicantSurName"
										placeholder="Sur-Name of Advertiser/Applicant">
								</div>
							</div>
						</div>
						<div class="col-md-6">
							<div class="form-group">
								<label class="col-md-6 control-label" for="textinput">Name
									of Advertiser/Applicant</label>
								<div class="col-md-6" style="margin-bottom: 15px;">
									<input class="form-control" name="applicantName"
										id="applicantName" placeholder="Name of Advertiser/Applicant">
								</div>
							</div>
						</div>
					</div>

					<div class="col-md-12">
						<div class="col-md-6">
							<div class="form-group">
								<label class="col-md-6 control-label" for="textinput">
									PAN Number</label>
								<div class="col-md-6" style="margin-bottom: 15px;">
									<input class="form-control" placeholder="Advertiser PAN Number"
										name="panNumber" id="panNumber">
								</div>
							</div>
						</div>
						<div class="col-md-6">
							<div class="form-group">
								<label class="col-md-6 control-label" for="textinput">Aadhar
									Number</label>
								<div class="col-md-6" style="margin-bottom: 15px;">
									<input name="AadharNum" id="AadharNum"
										placeholder="Aadhar Number" class="form-control" type="text">
								</div>
							</div>
						</div>
					</div>
					<div class="col-md-12 control-label" align="center"
						id="errorBoxBasic"
						style="color: red; font-weight: bold; font-size: 15px;"></div>
				</div>
			</div>

			<div class="panel panel-default">
				<div class="panel-heading">Applicant Address Detail</div>
				<div class="panel-body">
					<div class="row">

						<div class="col-md-12">
							<div class="col-md-6">
								<div class="form-group">
									<label class="col-md-6 control-label" for="textinput">District
									</label>
									<div class="col-md-6" style="margin-bottom: 15px;">
										<select name="applicantDistrict" id="applicantDistrict"
											onchange="javascript:getListDistrictWise()"
											class="form-control">
											<option value="0">Select District</option>

											<c:forEach items="${requestScope.districtsList}"
												var="districtsList">
												<option value="<c:out value="${districtsList.districtId}"/>">
													<c:out value="${districtsList.districtName}" />
												</option>
											</c:forEach>
										</select>
									</div>
								</div>
							</div>
							<div class="col-md-6">
								<div class="form-group">
									<label class="col-md-6 control-label">ULB Name:</label>
									<div class="col-md-4 inputGroupContainer">
										<div class="input-group" id="ulbsTR">
											<!-- name="searchUlbId0" -->
											<select id="searchUlbId0" class="form-control ulbs">
												<option value="0">Select ULB</option>
											</select>
										</div>

										<div class="input-group" id="ulbsTR">
											<c:forEach var="subUlb" items="${requestScope.subUlbsMap}">
												<c:set var="subUlbsList" value="${subUlb.value}" />
												<select onchange="javascript:getlocality(this.value)"
													id="searchUlbId${subUlb.key}" class="form-control ulbs"
													style="display: none;" name="searchUlb">
													<option value="0">Select ULB</option>
													<c:forEach items="${subUlbsList}" var="subUlb">
														<option value="${subUlb.ulbCode}">
															<c:out value="${subUlb.ulbName}" />
														</option>
													</c:forEach>
												</select>
											</c:forEach>
										</div>
									</div>
								</div>
							</div>
						</div>

						<div class="col-md-12">
							<div class="col-md-6">
								<div class="form-group">
									<label class="col-md-6 control-label" for="textinput">Door
										Number</label>
									<div class="col-md-6" style="margin-bottom: 15px;">
										<input class="form-control" name="applicantDoorNumber"
											id="applicantDoorNumber" placeholder="Door Number">
									</div>
								</div>
							</div>
							<div class="col-md-6">
								<div class="form-group">
									<label class="col-md-6 control-label" for="textinput">Address
									</label>
									<div class="col-md-6" style="margin-bottom: 15px;">
										<input class="form-control" name="applicantAddressLine1"
											id="applicantAddressLine1" placeholder="Address line ">
									</div>
								</div>
							</div>

						</div>

						<div class="col-md-12">

							<div class="col-md-6">
								<div class="form-group">
									<label class="col-md-6 control-label" for="textinput">Locality
									</label>
									<div class="col-md-6" style="margin-bottom: 15px;">
										<!-- <input class="form-control" name="applicantLocality"
										id="applicantLocality" placeholder="Block Number"> -->

										<select class="form-control" name="applicantLocalityid"
											id="applicantLocalityid"
											onchange="javascript:getzone(this.value)">
											<option value="0">-Select-</option>

										</select>
									</div>
								</div>
							</div>
							<div class="col-md-6">
								<div class="form-group">
									<label class="col-md-6 control-label" for="textinput">Zone
									</label>
									<div class="col-md-6" style="margin-bottom: 15px;">

										<select class="form-control" name="applicantZoneid"
											id="applicantZoneid"
											onchange="javascript:getward(this.value)">
											<option value="0">-Select-</option>

										</select>
									</div>
								</div>
							</div>

						</div>


						<div class="col-md-12">

							<div class="col-md-6">
								<div class="form-group">
									<label class="col-md-6 control-label" for="textinput">Ward
									</label>
									<div class="col-md-6" style="margin-bottom: 15px;">
										<!-- <input class="form-control" name="applicantWard"
												id="applicantWard" placeholder="Ward"> -->
										<select class="form-control" name="applicantWardid"
											id="applicantWardid"
											onchange="javascript:getblock(this.value)">
											<option value="0">-Select-</option>
										</select>

									</div>
								</div>
							</div>
							<div class="col-md-6">
								<div class="form-group">

									<label class="col-md-6 control-label" for="textinput">Block
										Number </label>
									<div class="col-md-6" style="margin-bottom: 15px;">

										<select class="form-control" name="applicantBlockNumberid"
											id="applicantBlockNumberid"
											onchange="javascript:getstreet(this.value)">
											<option value="0">-Select-</option>
										</select>
									</div>
								</div>
							</div>

						</div>

						<div class="col-md-12">

							<div class="col-md-6">
								<div class="form-group">
									<label class="col-md-6 control-label" for="textinput">Street
									</label>
									<div class="col-md-6" style="margin-bottom: 15px;">
										<!-- <input class="form-control" name="applicantStreet"
												id="applicantStreet" placeholder="Street"> -->

										<select class="form-control" name="applicantStreetid"
											id="applicantStreetid"
											onchange="javascript:setstreet(this.value)">
											<option value="0">-Select-</option>

										</select>


									</div>
								</div>
							</div>
							<div class="col-md-6">
								<div class="form-group">
									<label class="col-md-6 control-label" for="textinput">City
									</label>
									<div class="col-md-6" style="margin-bottom: 15px;">
										<input class="form-control" name="applicantCity"
											id="applicantCity" placeholder="City">
									</div>
								</div>
							</div>

						</div>


						<div class="col-md-12 control-label" align="center"
							id="errorBoxapplicationaddress"
							style="color: red; font-weight: bold; font-size: 15px;"></div>
					</div>
				</div>
			</div>

			<div class="panel panel-default">
				<div class="panel-heading">Applicant Contact Detail</div>
				<div class="panel-body">
					<div class="row">
						<div class="col-md-12">
							<div class="col-md-6">
								<div class="form-group">
									<label class="col-md-6 control-label" for="textinput">Mobile
										Number</label>
									<div class="col-md-6" style="margin-bottom: 15px;">
										<input class="form-control" name="mobileNumber"
											id="mobileNumber" placeholder="Mobile Number">
									</div>
								</div>
							</div>
							<div class="col-md-6">
								<div class="form-group">
									<label class="col-md-6 control-label" for="textinput">Email
										Id </label>
									<div class="col-md-6" style="margin-bottom: 15px;">
										<input class="form-control" name="emailId" id="emailId"
											placeholder="Email Id">
									</div>
								</div>
							</div>
						</div>

						<div class="col-md-12">
							<div class="col-md-6">
								<div class="form-group">
									<!-- <label class="col-md-6 control-label" for="textinput">STD
									Code </label>
								<div class="col-md-6" style="margin-bottom: 15px;">
									<input class="form-control" name="stdCode" id="stdCode"
										placeholder="STD Code">
								</div> -->
								</div>
							</div>
							<div class="col-md-6">
								<div class="form-group">
									<!-- <label class="col-md-6 control-label" for="textinput">Phone
									Number </label>
								<div class="col-md-6" style="margin-bottom: 15px;">
									<input class="form-control" name="phoneNumber" id="phoneNumber"
										placeholder="Phone Number">
								</div> -->
								</div>
							</div>
						</div>
						<!-- <div class="col-md-12">

						<div class="col-md-6">
							<div class="form-group">
								<label class="col-md-6 control-label" for="textinput">Fax
									Number </label>
								<div class="col-md-6" style="margin-bottom: 15px;">
									<input class="form-control" name="faxNumber" id="faxNumber"
										placeholder="Fax Number">
								</div>
							</div>
						</div>
						<div class="col-md-6">
							<div class="form-group" style="display: none">
								 
							</div>
						</div>
					</div> -->
						<div class="col-md-12 control-label" align="center"
							id="errorBoxContact"
							style="color: red; font-weight: bold; font-size: 15px;"></div>
					</div>
				</div>
			</div>

			<div class="panel panel-default">
				<div class="panel-heading" style="">Advertisement Location
					Details</div>
				<div class="panel-body">
					<div class="row">
						<div class="col-md-12">
							<div class="col-md-6">
								<div class="form-group">
									<label class="col-md-6 control-label" for="textinput">LandMark
										of Building/Premises</label>
									<div class="col-md-6" style="margin-bottom: 15px;">
										<input class="form-control" name="landmark" id="landmark"
											placeholder="LandMark">
									</div>
								</div>
							</div>
							<div class="col-md-6">
								<div class="form-group">
									<label class="col-md-6 control-label" for="textinput">Owner
										Name(Building/Premises) </label>
									<div class="col-md-6" style="margin-bottom: 15px;">
										<input class="form-control" name="buildingOwnerName"
											id="buildingOwnerName"
											placeholder="Owner Name of Building/Premises">
									</div>
								</div>
							</div>
						</div>

						<div class="col-md-12">
							<div class="col-md-6">
								<div class="form-group">
									<label class="col-md-6 control-label" for="textinput">Door
										Number </label>
									<div class="col-md-6" style="margin-bottom: 15px;">
										<input class="form-control" name="buildingDoorNumber"
											id="buildingDoorNumber" placeholder="Door Number">
									</div>
								</div>
							</div>
							<div class="col-md-6">
								<div class="form-group">
									<label class="col-md-6 control-label" for="textinput">Address
									</label>
									<div class="col-md-6" style="margin-bottom: 15px;">
										<input class="form-control" name="buildingAddress1"
											id="buildingAddress1"
											placeholder="Building/Premises Address-1">
									</div>
								</div>
							</div>
						</div>

						<div class="col-md-12">
							<div class="col-md-6">
								<div class="form-group">
									<label class="col-md-6 control-label" for="textinput">Building
										Assessment Number</label>
									<div class="col-md-6" style="margin-bottom: 15px;">
										<input class="form-control" name="buildingAssnmtNumber"
											id="buildingAssnmtNumber"
											placeholder="Building/Premises Assessment Number">
									</div>
									<!-- <label class="col-md-6 control-label" for="textinput">Address-2
								</label>
								<div class="col-md-6" style="margin-bottom: 15px;">
									<input class="form-control" name="buildingAddress2"
										id="buildingAddress2"
										placeholder="Building/Premises Address-2">
								</div> -->
								</div>
							</div>
							<div class="col-md-6">
								<div class="form-group">
									<label class="col-md-6 control-label" for="textinput">City</label>
									<div class="col-md-6" style="margin-bottom: 15px;">
										<input class="form-control" name="buildingCity"
											id="buildingCity" placeholder="Building/Premises City">
									</div>
								</div>
							</div>
						</div>

						<div class="col-md-12">
							<div class="col-md-6">
								<div class="form-group">
									<div class="form-group">
										<!-- <label class="col-md-6 control-label" for="textinput">Subject
									Matter </label> -->
										<div class="col-md-6" style="margin-bottom: 15px;">
											<!--<input class="form-control" name="subjectMatter"
										id="subjectMatter" placeholder="Subject Matter"> -->
										</div>
									</div>
								</div>
							</div>
							<div class="col-md-6">
								<div class="form-group"></div>
							</div>
						</div>


						<div class="col-md-12 control-label" align="center"
							id="errorBoxlocation"
							style="color: red; font-weight: bold; font-size: 15px;"></div>
					</div>
				</div>
			</div>

			<!-- Hoarding Category Detail Starts-->
			<div class="panel panel-default">
				<div class="panel-heading">Advertisement Category</div>
				<div class="panel-body">
					<div class="row">
						<div class="col-md-12">
							<div class="col-md-6">
								<div class="form-group">
									<label class="col-md-6 control-label" for="textinput">Advertisement
										Category</label>
									<div class="col-md-6" style="margin-bottom: 15px;">
										<select class="form-control" name="advertisementCategory"
											id="advertisementCategory"
											onchange="javascript:getSubCategory(this.value)">
											<option value="0">-Select Category-</option>

										</select>
									</div>
								</div>
							</div>
							<div class="col-md-6">
								<div class="form-group">
									<label class="col-md-6 control-label" for="textinput">Advertisement
										Sub Category</label>
									<div class="col-md-6" style="margin-bottom: 15px;">
										<select class="form-control" name="advertisementSubCategory"
											id="advertisementSubCategory"
											onchange="javascript:getAmtNonHoardingAdvr(this.value)">
											<option value="0">-Select Category-</option>
											<option value="1">-sub1-</option>
										</select>
									</div>
								</div>
							</div>
						</div>

						<div class="col-md-12">
							<div class="col-md-6">
								<div class="form-group">
									<label class="col-md-6 control-label" for="textinput">Unit
										Name </label>
									<div class="col-md-6" style="margin-bottom: 15px;">
										<input class="form-control" name="unitName" id="unitName"
											placeholder="Unit Name ">
									</div>
								</div>
							</div>
							<div class="col-md-6">
								<div class="form-group">
									<label class="col-md-6 control-label" for="textinput">Land
										OwnerShip </label>
									<div class="col-md-6" style="margin-bottom: 15px;">

										<select class="form-control" name="landOwnerShip"
											id="landOwnerShip">
											<option value="0">-Select-</option>
											<option value="1">Central Govt. Building</option>
											<option value="2">House Owned Under EWSHS</option>
											<option value="3">State Govt. Building</option>
											<option value="4">Private Buildings</option>
											<option value="5">Private Buildings-Companies</option>
										</select>
									</div>
								</div>
							</div>
						</div>
						<div class="col-md-12 control-label" align="center"
							id="errorBoxCategory"
							style="color: red; font-weight: bold; font-size: 15px;"></div>
					</div>
				</div>
			</div>
			<!-- Hoarding Category Detail End -->

			<!-- Non Hording Detail Start -->
			<div class="panel panel-default">
				<div class="panel-heading">Non Hoarding Details</div>
				<div class="panel-body">
					<div class="row">
						<div class="col-md-12">
							<div class="col-md-6">
								<div class="form-group">
									<label class="col-md-6 control-label" for="textinput">Length
										in mts/Nos</label>
									<div class="col-md-6" style="margin-bottom: 15px;">
										<input class="form-control" name="lengthInMtsNos"
											id="lengthInMtsNos" placeholder="Length in mts/Nos"
											onkeyup="javascript:getTotalArea()">
									</div>
								</div>
							</div>
							<div class="col-md-6">
								<div class="form-group">
									<label class="col-md-6 control-label" for="textinput">Width
										in mts</label>
									<div class="col-md-6" style="margin-bottom: 15px;">
										<input class="form-control" name="widthInMtsNos"
											id="widthInMtsNos" placeholder="Width in mts/Nos"
											onkeyup="javascript:getTotalArea()">
									</div>
								</div>
							</div>
						</div>

						<div class="col-md-12">
							<div class="col-md-6">
								<div class="form-group">
									<label class="col-md-6 control-label" for="textinput">Total
										Area</label>
									<div class="col-md-6" style="margin-bottom: 15px;">
										<input class="form-control" name="advertiseTotalArea"
											id="advertiseTotalArea" placeholder="Total Area"
											onkeyup="javascript:getTotalArea()" readonly="readonly">
									</div>
								</div>
							</div>
							<div class="col-md-6">
								<div class="form-group">
									<label class="col-md-6 control-label" for="textinput">Details</label>
									<div class="col-md-6" style="margin-bottom: 15px;">
										<input class="form-control" name="nonHoardingDetails"
											id="nonHoardingDetails" placeholder="Details">
									</div>
								</div>
							</div>
						</div>
						<div class="col-md-12 control-label" align="center"
							id="errorBoxnonHoarding"
							style="color: red; font-weight: bold; font-size: 15px;"></div>

					</div>
				</div>
			</div>
			<!-- Non Hording Detail End -->

			<!-- Advertisement Detail Start -->
			<div class="panel panel-default">
				<div class="panel-heading">Advertisement Details</div>
				<div class="panel-body">
					<div class="row">
						<div class="col-md-12">
							<div class="col-md-6">
								<div class="form-group">
									<label class="col-md-6 control-label" for="textinput">Start
										Date</label>
									<div class="col-md-6" style="margin-bottom: 15px;">
										<input class="form-control" name="startDate" id="startDate"
											placeholder="Start Date" readonly="readonly">
									</div>
								</div>
							</div>
							<div class="col-md-6">
								<div class="form-group">
									<label class="col-md-6 control-label" for="textinput">End
										Date</label>
									<div class="col-md-6" style="margin-bottom: 15px;">
										<input class="form-control" name="endDate" id="endDate"
											placeholder="End Date" readonly="readonly">
									</div>
								</div>
							</div>
						</div>

						<div class="col-md-12">

							<div class="col-md-6">
								<div class="form-group">
									<label class="col-md-6 control-label" for="textinput">Facing</label>
									<div class="col-md-6" style="margin-bottom: 15px;">
										<select class="form-control" tabindex="23" name="facing"
											id="facing">
											<option value="0">Select Facing</option>
											<option value="N">North</option>
											<option value="S">South</option>
											<option value="E">East</option>
											<option value="W">West</option>
											<option value="ES">East and South</option>
											<option value="EN">East and North</option>
											<option value="WS">West and South</option>
											<option value="WN">West and North</option>
										</select>

									</div>
								</div>
							</div>
							<div class="col-md-6">
								<div class="form-group">
									<label class="col-md-6 control-label" for="textinput">Any
										Other PartiCulars</label>
									<div class="col-md-6" style="margin-bottom: 15px;">
										<input class="form-control" name="otherParticulars"
											id="otherParticulars" placeholder="PartiCulars">
									</div>
								</div>
							</div>

						</div>
						<div class="col-md-12 control-label" align="center"
							id="errorBoxadvdetails"
							style="color: red; font-weight: bold; font-size: 15px;"></div>
					</div>
				</div>
			</div>
			<!-- Advertisement Detail End -->

			<div class="panel-body">
				<div class="row">
					<div class="form-group">
						<div class="col-md-12 text-center">
							<!-- <input class="form-control success" type="submit" value="Save Details"> -->
							<div class="form-group">
								<!-- <label class="col-md-2 control-label" for="button1id"></label> -->
								<div class="col-md-12 text-center" id="buttonsSaveReset">
									<input type="submit" value="&nbsp;&nbsp; Save &nbsp;&nbsp;"
										class="btn btn-success" name="submitAdvr" id="submitAdvr">
									<input type="reset" value="&nbsp;&nbsp;Reset&nbsp;&nbsp;"
										class="btn btn-danger"> <br>
								</div>
								<div class="col-md-12 control-label" align="center"
									id="amtErrBox"
									style="color: red; font-weight: bold; font-size: 15px;">
									<br>
								</div>
								<!-- <label class="col-md-2 control-label" ></label> -->
							</div>
						</div>
					</div>
				</div>
			</div>

		</div>

		<input type="hidden" name="applicantUlb" id="applicantUlb" value="">
		<input type="hidden" name="applicantLocality" id="applicantLocality"
			value=""> <input type="hidden" name="applicantZone"
			id="applicantZone" value=""> <input type="hidden"
			name="applicantWard" id="applicantWard" value=""> <input
			type="hidden" name="applicantBlockNumber" id="applicantBlockNumber"
			value=""> <input type="hidden" name="applicantStreet"
			id="applicantStreet" value=""> <input type="hidden"
			id="advertisementAmt" name="advertisementAmt" value="0">


	</fieldset>
</form>



<link rel="stylesheet" href="<%=path%>/jsp/css/jquery-ui.css">
<script src="<%=path%>/jsp/js/jquery-1.9.1.js"></script>
<script src="<%=path%>/jsp/js/jquery-ui.js"></script>

<script type="text/javascript">
	function checkValidation() {

		//alert("asdcsfsd");
		var name_Regx = /^[a-zA-Z ]{2,50}$/;
		var mobile_reg = /^[6-9]+[0-9]{9}$/;
		var rex_pannumber = /^[A-Z]{3}[CHFATBLJGP]{1}[A-Z]{1}[0-9]{4}[A-Z]$/;
		var email_Regx = /[a-z0-9._%+-]+@[a-z0-9.-]+\.[a-z]{2,3}$/;
		var number_Regx = /^[0-9]+$/;
		var doublecheck_reg = /^\d{0,2}(?:\.\d{0,2}){0,1}$/;
		var age_Regx = /^[0-9]{1,2}$/;
		var assessment = /^[1]+[0-9]{9}$/;

		var applicantSurName = document.advAppForm.applicantSurName.value;
		var applicantName = document.advAppForm.applicantName.value;
		var panNumber = document.advAppForm.panNumber.value;
		var AadharNum = document.advAppForm.AadharNum.value;
		var applicantDoorNumber = document.advAppForm.applicantDoorNumber.value;
		var applicantAddressLine1 = document.advAppForm.applicantAddressLine1.value;
		var applicantCity = document.advAppForm.applicantCity.value;
		var applicantBlockNumber = document.advAppForm.applicantBlockNumber.value;
		var applicantZone = document.advAppForm.applicantZone.value;
		var applicantLocality = document.advAppForm.applicantLocality.value;
		var applicantWard = document.advAppForm.applicantWard.value;
		var applicantStreet = document.advAppForm.applicantStreet.value;
		var applicantDistrict = document.advAppForm.applicantDistrict.value;
		var applicantUlb = document.advAppForm.applicantUlb.value;
		var mobileNumber = document.advAppForm.mobileNumber.value;
		var emailId = document.advAppForm.emailId.value;
		//var stdCode
		//var phoneNumber
		var landmark = document.advAppForm.landmark.value;
		var buildingOwnerName = document.advAppForm.buildingOwnerName.value;
		var buildingDoorNumber = document.advAppForm.buildingDoorNumber.value;
		var buildingAddress1 = document.advAppForm.buildingAddress1.value;
		var buildingCity = document.advAppForm.buildingCity.value;
		//var buildingPinCode
		var buildingAssnmtNumber = document.advAppForm.buildingAssnmtNumber.value; //-opt
		var advertisementCategory = document.advAppForm.advertisementCategory.value;
		var advertisementSubCategory = document.advAppForm.advertisementSubCategory.value;
		var unitName = document.advAppForm.unitName.value;
		var landOwnerShip = document.advAppForm.landOwnerShip.value;
		var lengthInMtsNos = document.advAppForm.lengthInMtsNos.value;
		var widthInMtsNos = document.advAppForm.widthInMtsNos.value;
		var advertiseTotalArea = document.advAppForm.advertiseTotalArea.value;
		var nonHoardingDetails = document.advAppForm.nonHoardingDetails.value;
		var startDate = document.advAppForm.startDate.value;
		var endDate = document.advAppForm.endDate.value;
		var otherParticulars = document.advAppForm.otherParticulars.value; //-opt
		var facing = document.advAppForm.facing.value;

		//var startDate=document.advAppForm.startDate.value;
		//var endDate=document.advAppForm.endDate.value;

		var issubmit = false;

		if (!name_Regx.test(applicantSurName)) {
			document.getElementById("errorBoxBasic").innerHTML = "Only Alphbets are allowed in Sur-Name ";
			//document.getElementById("ownerSurName").value = "";
			document.getElementById("applicantSurName").focus();
			return false;
		}
		if (!name_Regx.test(applicantName)) {
			document.getElementById("errorBoxBasic").innerHTML = "Only Alphbets are allowed in Name ";
			//document.getElementById("ownerSurName").value = "";
			document.getElementById("applicantName").focus();
			return false;
		}

		if (!rex_pannumber.test(panNumber) && panNumber != '') {
			//alert("invalid Pan");
			document.getElementById("errorBoxBasic").innerHTML = "Please Enter Valid Pan Number";
			document.getElementById("panNumber").value = "";
			document.getElementById("panNumber").focus();
			return false;
		}

		if ((AadharNum != '' && !number_Regx.test(AadharNum))) {
			document.getElementById("AadharNum").value = "";
			document.getElementById("AadharNum").focus();//alert(verhoeffLookup(ownerAadhar, 0));
			document.getElementById("errorBoxBasic").innerHTML = "Please Enter  Valid Aadhar Number";
			return false;
		}

		if (verhoeffLookup(AadharNum, 0) != 0) {
			document.getElementById("AadharNum").value = "";
			document.getElementById("AadharNum").focus();
			document.getElementById("errorBoxBasic").innerHTML = "Please Enter  Valid Aadhar Number";
			return false;
		}
		document.getElementById("errorBoxBasic").innerHTML = "";

		//document.getElementById("applicantUlb").value = ulbid;

		if (applicantDistrict == '0' || applicantDistrict == null
				|| applicantDistrict == '') {
			document.getElementById("errorBoxapplicationaddress").innerHTML = "Please Select Your District ";
			document.getElementById("applicantDistrict").focus();
			return false;
		}

		if (applicantUlb == '0' || applicantUlb == null || applicantUlb == '') {
			document.getElementById("errorBoxapplicationaddress").innerHTML = "Please Select Your ULB ";
			return false;
		}
		if (applicantDoorNumber == '' || applicantDoorNumber == null) {
			document.getElementById("errorBoxapplicationaddress").innerHTML = "Please Enter Door Number";
			//document.getElementById("ownerAadhar").value = "";
			document.getElementById("applicantDoorNumber").focus();
			return false;
		}

		if (applicantAddressLine1 == '' || applicantAddressLine1 == null) {
			document.getElementById("errorBoxapplicationaddress").innerHTML = "Please Enter Address ";
			//document.getElementById("ownerVilliage").value = "";
			document.getElementById("applicantAddressLine1").focus();
			return false;
		}
		
		if (applicantAddressLine1.length>25) {
			document.getElementById("errorBoxapplicationaddress").innerHTML = "Maximum 25 Character Allowed in Address";
			//document.getElementById("ownerVilliage").value = "";
			document.getElementById("applicantAddressLine1").focus();
			return false;
		}

		if (applicantLocality == '0' || applicantLocality == null
				|| applicantLocality == '') {
			document.getElementById("errorBoxapplicationaddress").innerHTML = "Please Select Your Locality ";
			//document.getElementById("applicantLocality").focus();
			return false;
		}
		if (applicantZone == '0' || applicantZone == null
				|| applicantZone == '') {
			document.getElementById("errorBoxapplicationaddress").innerHTML = "Please Select Your Zone ";
			document.getElementById("applicantZone").focus();
			return false;
		}

		if (applicantWard == '0' || applicantWard == null
				|| applicantWard == '') {
			document.getElementById("errorBoxapplicationaddress").innerHTML = "Please Select Your Revenue Ward ";
			document.getElementById("applicantWard").focus();
			return false;
		}

		if (applicantBlockNumber == '0' || applicantBlockNumber == null
				|| applicantBlockNumber == '') {
			document.getElementById("errorBoxapplicationaddress").innerHTML = "Please Select Your Block ";
			document.getElementById("applicantBlockNumber").focus();
			return false;
		}

		if (!name_Regx.test(applicantCity)) {
			document.getElementById("errorBoxapplicationaddress").innerHTML = "Please Enter City ";
			//document.getElementById("ownerVilliage").value = "";
			document.getElementById("applicantCity").focus();
			return false;
		}

		document.getElementById("errorBoxapplicationaddress").innerHTML = "";
		if (!mobile_reg.test(mobileNumber)) {
			document.getElementById("errorBoxContact").innerHTML = "Please Enter Valid Mobile Number ";
			document.getElementById("mobileNumber").value = "";
			document.getElementById("mobileNumber").focus();
			return false;
		}

		if (emailId != '' && !email_Regx.test(emailId)) {
			document.getElementById("errorBoxContact").innerHTML = "Please Enter Valid Email Id";
			document.getElementById("emailId").value = "";
			document.getElementById("emailId").focus();
			return false;
		}

		document.getElementById("errorBoxContact").innerHTML = "";
		//errorBoxlocation
		if (!name_Regx.test(landmark)) {
			document.getElementById("errorBoxlocation").innerHTML = "Please Enter LandMark";
			//document.getElementById("ownerVilliage").value = "";
			document.getElementById("landmark").focus();
			return false;
		}

		if (!name_Regx.test(buildingOwnerName)) {
			document.getElementById("errorBoxlocation").innerHTML = "Please Enter Bulding OwnerName";
			//document.getElementById("ownerVilliage").value = "";
			document.getElementById("buildingOwnerName").focus();
			return false;
		}
		if (buildingDoorNumber.length == 0 || buildingDoorNumber == null
				|| buildingDoorNumber == '') {
			document.getElementById("errorBoxlocation").innerHTML = "Please Enter Bulding Door Number";
			//document.getElementById("ownerAadhar").value = "";
			document.getElementById("buildingDoorNumber").focus();
			return false;
		}

		if (buildingAddress1 == '' || buildingAddress1 == null) {
			document.getElementById("errorBoxlocation").innerHTML = "Please Enter Bulding Address";
			//document.getElementById("ownerAadhar").value = "";
			document.getElementById("buildingAddress1").focus();
			return false;
		}
		
		if (buildingAddress1.length>25) {
			document.getElementById("errorBoxlocation").innerHTML = "Maximum 25 Character Allowed in Address";
			//document.getElementById("ownerVilliage").value = "";
			document.getElementById("buildingAddress1").focus();
			return false;
		}
		//buildingAssnmtNumber

		if (!assessment.test(buildingAssnmtNumber)
				&& buildingAssnmtNumber != '') {
			document.getElementById("errorBoxlocation").innerHTML = "Enter valid Property Tax Assessment Number..";
			document.getElementById("buildingAssnmtNumber").focus();
			return false;
		}

		if (!name_Regx.test(buildingCity)) {
			document.getElementById("errorBoxlocation").innerHTML = "Please Enter Building City ";
			//document.getElementById("ownerVilliage").value = "";
			document.getElementById("buildingCity").focus();
			return false;
		}
		document.getElementById("errorBoxlocation").innerHTML = "";
		//advertisementCategory,advertisementSubCategory,unitName

		if (advertisementCategory == '0' || advertisementCategory == null
				|| advertisementCategory == '') {
			document.getElementById("errorBoxCategory").innerHTML = "Please Select Advertisement Category  ";
			return false;
		}

		if (advertisementSubCategory == '0' || advertisementSubCategory == null
				|| advertisementSubCategory == '') {
			document.getElementById("errorBoxCategory").innerHTML = "Please Select  Advertisement Sub Category ";
			return false;
		}
		if (!name_Regx.test(unitName)) {
			document.getElementById("errorBoxCategory").innerHTML = "Please Enter Unit Name ";
			//document.getElementById("unitName").value = "";
			document.getElementById("unitName").focus();
			return false;
		}
		if (landOwnerShip == '0' || landOwnerShip == null
				|| landOwnerShip == '') {
			document.getElementById("errorBoxCategory").innerHTML = "Please Select  OwnerShip ";
			return false;
		}

		document.getElementById("errorBoxCategory").innerHTML = "";

		//errorBoxmeasurmentswidthInMtsNos  "lengthInMtsNos"
		if (lengthInMtsNos.length == 0 || !doublecheck_reg.test(lengthInMtsNos)) {
			document.getElementById("errorBoxnonHoarding").innerHTML = "Please Enter Length  ";
			document.getElementById("lengthInMtsNos").value = "";
			document.getElementById("lengthInMtsNos").focus();
			return false;
		}
		if (widthInMtsNos.length == 0 || !doublecheck_reg.test(widthInMtsNos)) {
			document.getElementById("errorBoxnonHoarding").innerHTML = "Please Enter Width  ";
			document.getElementById("widthInMtsNos").value = "";
			document.getElementById("widthInMtsNos").focus();
			return false;
		}

		if (advertiseTotalArea.length == 0
				|| !doublecheck_reg.test(advertiseTotalArea)) {
			document.getElementById("errorBoxnonHoarding").innerHTML = "Please Enter Length and Width ";
			
			if (lengthInMtsNos.length == 0
					|| !doublecheck_reg.test(lengthInMtsNos)) {
				document.getElementById("errorBoxnonHoarding").innerHTML = "Please Enter Length  ";
				document.getElementById("lengthInMtsNos").value = "";
				document.getElementById("lengthInMtsNos").focus();
				return false;
			}
			if (widthInMtsNos.length == 0
					|| !doublecheck_reg.test(widthInMtsNos)) {
				document.getElementById("errorBoxnonHoarding").innerHTML = "Please Enter Width  ";
				document.getElementById("widthInMtsNos").value = "";
				document.getElementById("widthInMtsNos").focus();
				return false;
			}

			return false;
		}

		if (!name_Regx.test(nonHoardingDetails) && nonHoardingDetails != '') {
			document.getElementById("errorBoxnonHoarding").innerHTML = "Please Enter Valid Non-Hoarding Details ";
			document.getElementById("nonHoardingDetails").value = "";
			document.getElementById("nonHoardingDetails").focus();
			return false;
		}

		document.getElementById("errorBoxnonHoarding").innerHTML = "";
		if (startDate == null || startDate == '') {
			document.getElementById("startDate").focus();
			document.getElementById("startDate").value = "";
			document.getElementById("errorBoxadvdetails").innerHTML = "Please Enter Start Date";
			return false;
		}
		if (endDate == null || endDate == '') {
			document.getElementById("endDate").focus();
			document.getElementById("endDate").value = "";
			document.getElementById("errorBoxadvdetails").innerHTML = "Please Enter End Date";
			return false;
		}

		if (facing == 0 || facing == null || facing == '') {
			document.getElementById("errorBoxadvdetails").innerHTML = "Please Select Facing ";
			document.getElementById("facing").focus();
			return false;
		}

		document.getElementById("errorBoxadvdetails").innerHTML = "";
		
		var advrAmt = document.getElementById("advertisementAmt").value;
		if (advrAmt == 0 || advrAmt == '') {
			document.getElementById("amtErrBox").innerHTML = "Amount not fixed for your selection criteria.";
			return false;
		}

		
		issubmit = true;

		if (issubmit) {
			$('#submitAdvr').prop('disabled', true);
		}

	}

	function verhoeffLookup(aNum, aOffset) {

		var d = [ [ 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 ],
				[ 1, 2, 3, 4, 0, 6, 7, 8, 9, 5 ],
				[ 2, 3, 4, 0, 1, 7, 8, 9, 5, 6 ],
				[ 3, 4, 0, 1, 2, 8, 9, 5, 6, 7 ],
				[ 4, 0, 1, 2, 3, 9, 5, 6, 7, 8 ],
				[ 5, 9, 8, 7, 6, 0, 4, 3, 2, 1 ],
				[ 6, 5, 9, 8, 7, 1, 0, 4, 3, 2 ],
				[ 7, 6, 5, 9, 8, 2, 1, 0, 4, 3 ],
				[ 8, 7, 6, 5, 9, 3, 2, 1, 0, 4 ],
				[ 9, 8, 7, 6, 5, 4, 3, 2, 1, 0 ] ];

		var p = [ [ 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 ],
				[ 1, 5, 7, 6, 2, 8, 3, 0, 9, 4 ],
				[ 5, 8, 0, 3, 7, 9, 6, 1, 4, 2 ],
				[ 8, 9, 1, 6, 0, 4, 3, 5, 2, 7 ],
				[ 9, 4, 5, 3, 1, 2, 6, 8, 7, 0 ],
				[ 4, 2, 8, 6, 5, 7, 3, 9, 0, 1 ],
				[ 2, 7, 9, 3, 8, 0, 6, 4, 1, 5 ],
				[ 7, 0, 4, 6, 9, 1, 3, 2, 5, 8 ] ];

		var lookup = 0;
		for (i = aNum.length - 1; i >= 0; i = i - 1) {
			lookup = d[lookup][p[(aNum.length - 1 - i + aOffset) % 8][parseInt(aNum
					.substr(i, 1))]];
		}

		return lookup;
	}
</script>

<script type="text/javascript">
	function setulb(applicantUlb) {
		//alert(applicantUlb);
		document.getElementById("applicantUlb").value = applicantUlb;
	}

	function getlocality(ulbid) {

		var option = $('<option />').val("0").text("-Select-");
		$("#applicantLocalityid").empty();
		document.getElementById("applicantUlb").value = ulbid;
			
		//if (ulbid == 0) {
			$("#applicantLocalityid").empty();
			 $("#applicantLocalityid").append(
					$('<option />').val("0").text("-Select-")); 

			$("#applicantZoneid").empty();
			 $("#applicantZoneid").append(
					$('<option />').val("0").text("-Select-"));  

			$("#applicantWardid").empty();
			$("#applicantWardid").append(
					$('<option />').val("0").text("-Select-")); 

			$("#applicantBlockNumberid").empty();
			 $("#applicantBlockNumberid").append(
					$('<option />').val("0").text("-Select-")); 

			$("#applicantStreetid").empty();
			 $("#applicantStreetid").append(
					$('<option />').val("0").text("-Select-"));			 

			//return false;
		//}
			if (ulbid == 0) {
				document.getElementById("applicantUlb").value = '0';
				getCategory(ulbid);
				getAmtNonHoardingAdvr();
				return false;
			}
		var url = "${pageContext.request.contextPath}/getlocality.do";

		//$("#applicantLocalityid").append(option);
		$.ajax({

			type : "POST",
			url : url,
			data : {
				ulbcode : ulbid
			},

			dataType : 'json',
			success : function(data) {
				var array = data;
				$.each(array, function(i, elem) {

					option = $('<option />').val(i).text(elem);
					$("#applicantLocalityid").append(option);

				});
			},
			error : function() {
				var soption = $('<option />').val("0").text("select");
				$("#applicantLocalityid").append(soption);
			}
		});

		getCategory(ulbid);
		getAmtNonHoardingAdvr();
	}

	function getzone(localityid) {
		
		document.getElementById("applicantLocality").value = localityid;

		var option = $('<option />').val("0").text("-Select-");
		$("#applicantZoneid").empty();

		var ulbid = document.getElementById("applicantUlb").value;
		//if (ulbid == 0 || localityid == 0) {

			$("#applicantZoneid").empty();
			$("#applicantZoneid").append(
					$('<option />').val("0").text("-Select-"));

			$("#applicantWardid").empty();
			$("#applicantWardid").append(
					$('<option />').val("0").text("-Select-"));

			$("#applicantBlockNumberid").empty();
			$("#applicantBlockNumberid").append(
					$('<option />').val("0").text("-Select-"));

			$("#applicantStreetid").empty();
			$("#applicantStreetid").append(
					$('<option />').val("0").text("-Select-"));

			//return false;
		//}
			if (ulbid == 0 || localityid == 0) {
				return false;
				}
			
		var url = "${pageContext.request.contextPath}/getzone.do";

		//$("#applicantZoneid").append(option);
		$.ajax({

			type : "POST",
			url : url,
			data : {
				ulbcode : ulbid,
				i_lctyobjid : localityid
			},
			dataType : 'json',
			success : function(data) {
				var array = data;

				$.each(array, function(i, elem) {

					option = $('<option />').val(i).text(elem);
					$("#applicantZoneid").append(option);

				});

			},
			error : function() {
				var soption = $('<option />').val("0").text("select");
				$("#applicantZoneid").append(soption);
			}
		});

	}

	function getward(zoneid) {

		document.getElementById("applicantZone").value = zoneid;

		var localityid = document.getElementById("applicantLocality").value;
		var ulbid = document.getElementById("applicantUlb").value;

		var option = $('<option />').val("0").text("select");
		$("#applicantWardid").empty();

		//if (ulbid == 0 || localityid == 0 || zoneid == 0) {

			$("#applicantWardid").empty();
			$("#applicantWardid").append(
					$('<option />').val("0").text("-Select-"));

			$("#applicantBlockNumberid").empty();
			$("#applicantBlockNumberid").append(
					$('<option />').val("0").text("-Select-"));

			$("#applicantStreetid").empty();
			$("#applicantStreetid").append(
					$('<option />').val("0").text("-Select-"));

			//return false;
		//}
			if (ulbid == 0 || localityid == 0 || zoneid == 0) {
				return false;
			}

		var url = "${pageContext.request.contextPath}/getward.do";

		//$("#applicantWardid").append(option);
		$.ajax({

			type : "POST",
			url : url,
			data : {
				localityid : localityid,
				ulbcode : ulbid
			},

			dataType : 'json',
			success : function(data) {
				var array = data;

				$.each(array, function(i, elem) {

					option = $('<option />').val(i).text(elem);
					$("#applicantWardid").append(option);

				});
			},
			error : function() {
				var soption = $('<option />').val("0").text("select");
				$("#applicantWardid").append(soption);
			}
		});

		//return false;
	}

	function getblock(reward) {
		//alert(">>getblck" + reward);
		document.getElementById("applicantWard").value = reward;
		var ulbid = document.getElementById("applicantUlb").value;
		var localityid = document.getElementById("applicantLocality").value;
		var zoneid = document.getElementById("applicantZone").value;

		var option = $('<option />').val("0").text("select");
		$("#applicantBlockNumberid").empty();

		//var zone = document.getElementById("block").value;

		/* document.getElementById("tradeUlb").innerHTML = ulbid; */
		//if (ulbid == 0 || reward == 0 || zoneid == 0 || localityid == 0) {
			//if (ulbid == 0 || localityid == 0) {

			$("#applicantBlockNumberid").empty();
			$("#applicantBlockNumberid").append(
					$('<option />').val("0").text("-Select-"));

			$("#applicantStreetid").empty();
			$("#applicantStreetid").append(
					$('<option />').val("0").text("-Select-"));

			//return false;
		//}
			if (ulbid == 0 || reward == 0 || zoneid == 0 || localityid == 0) {
			return false;
			}

		var url = "${pageContext.request.contextPath}/getblock.do";

		//$("#applicantBlockNumberid").append(option);
		$.ajax({

			type : "POST",
			url : url,
			data : {
				localityid : localityid,
				ulbcode : ulbid
			},

			dataType : 'json',
			success : function(data) {
				var array = data;

				$.each(array, function(i, elem) {

					option = $('<option />').val(i).text(elem);

					$("#applicantBlockNumberid").append(option);

				});

			},
			error : function() {
				var soption = $('<option />').val("0").text("select");
				$("#applicantBlockNumberid").append(soption);
			}
		});

		//return false;
	}

	function getstreet(blockid) {

		document.getElementById("applicantBlockNumber").value = blockid;
		var ulbid = document.getElementById("applicantUlb").value;
		var localityid = document.getElementById("applicantLocality").value;
		var zoneid = document.getElementById("applicantZone").value;
		var wardid = document.getElementById("applicantWard").value;

		var option = $('<option />').val("0").text("select");
		$("#applicantStreetid").empty();

		//if (ulbid == 0 || wardid == 0 || zoneid == 0 || blockid == 0 || localityid == 0) {
			//if (ulbid == 0 || localityid == 0) {
			$("#applicantStreetid").empty();
			$("#applicantStreetid").append(
					$('<option />').val("0").text("-Select-"));

			//return false;
		//}
			if (ulbid == 0 || wardid == 0 || zoneid == 0 || blockid == 0
					|| localityid == 0) {return false;}

		var url = "${pageContext.request.contextPath}/getstreet.do";
		//$("#applicantStreetid").append(option);
		$.ajax({

			type : "POST",
			url : url,
			data : {
				localityid : localityid,
				ulbcode : ulbid
			},

			dataType : 'json',
			success : function(data) {
				var array = data;

				$.each(array, function(i, elem) {

					option = $('<option />').val(i).text(elem);

					$("#applicantStreetid").append(option);

				});

			},
			error : function() {
				var soption = $('<option />').val("0").text("select");
				$("#applicantStreetid").append(soption);
			}
		});

		return false;
	}

	function getCategory(ulbid) {

		var ulbid = document.getElementById("applicantUlb").value;
		var option = $('<option />').val("0").text("select");
		$("#advertisementCategory").empty();

		//if (ulbid == 0) {
			//if (ulbid == 0 || localityid == 0) {
			$("#advertisementCategory").empty();
			$("#advertisementCategory").append(
					$('<option />').val("0").text("-Select-"));

			$("#advertisementSubCategory").empty();
			$("#advertisementSubCategory").append(
					$('<option />').val("0").text("-Select-"));

		//	return false;
		//}
			
		if (ulbid == 0) {
			return false;
		}
		var url = "${pageContext.request.contextPath}/getCategory.do";
		//$("#advertisementCategory").append(option);
		$.ajax({

			type : "POST",
			url : url,
			data : {
				ulbcode : ulbid
			},

			dataType : 'json',
			success : function(data) {
				var array = data;

				$.each(array, function(i, elem) {

					option = $('<option />').val(i).text(elem);

					$("#advertisementCategory").append(option);

				});

			},
			error : function() {
				var soption = $('<option />').val("0").text("select");
				$("#advertisementCategory").append(soption);
			}
		});

		return false;
	}

	function getSubCategory(catId) {

		var ulbid = document.getElementById("applicantUlb").value;
		var option = $('<option />').val("0").text("select");
		$("#advertisementSubCategory").empty();

		//if (ulbid == 0 || catId == 0) {
			$("#advertisementSubCategory").empty();
			$("#advertisementSubCategory").append(
					$('<option />').val("0").text("-Select-"));

		//	return false;
		//}
			if (ulbid == 0 || catId == 0) {return false;}

		var url = "${pageContext.request.contextPath}/getSubCategory.do";
		//$("#advertisementSubCategory").append(option);

		$.ajax({

			type : "POST",
			url : url,
			data : {
				categoryCode : catId,
				ulbcode : ulbid
			},

			dataType : 'json',
			success : function(data) {
				var array = data;

				$.each(array, function(i, elem) {

					option = $('<option />').val(i).text(elem);

					$("#advertisementSubCategory").append(option);

				});

			},
			error : function() {
				var soption = $('<option />').val("0").text("select");
				$("#advertisementSubCategory").append(soption);
			}
		});

		return false;
	}

	function setstreet(streetid) {
		document.getElementById("applicantStreet").value = streetid;

	}

	function getListDistrictWise() {
		$('.ulbs').hide();
		$('#ulbsTR').show();
		var ulbId = document.getElementById("applicantDistrict").value;
		if (ulbId != 0) {
			document.getElementById("searchUlbId" + ulbId).style.display = "block";
		} else {
			document.getElementById("searchUlbId0").style.display = "block";
		}

	}

	function getAmtNonHoardingAdvr(subcatId) {
		var ulbid = document.getElementById("applicantUlb").value;
		var totalarea = document.getElementById("advertiseTotalArea").value;
		var catId = document.advAppForm.advertisementCategory.value;

		var lenghtInMts = document.getElementById("lengthInMtsNos").value;
		var widthInMts = document.getElementById("widthInMtsNos").value;
		var totalArea = document.getElementById("advertiseTotalArea").value;

		if (ulbid == 0 || catId == 0 || subcatId == 0 || totalarea == 0
				|| lenghtInMts == 0 || lenghtInMts == '' || widthInMts == 0
				|| widthInMts == '' || totalArea == 0 || totalArea == '') {
			document.getElementById("buttonsSaveReset").style.visibility = 'hidden';
			//$('#submitAdvr').prop('disabled', true);
			return false;
		}

		document.getElementById("advertisementAmt").value = 0;
		var url = "${pageContext.request.contextPath}/getNonHoardingAdvrAmt.do";
		$
				.ajax({
					type : "POST",
					url : url,
					data : {
						i_ctgycode : catId,
						totarea : totalarea,
						i_subctgycode : subcatId,
						i_ulbobjid : ulbid
					},

					dataType : 'json',
					success : function(data) {
						document.getElementById("advertisementAmt").value = data;
						if (data == 0) {
							document.getElementById("buttonsSaveReset").style.visibility = 'hidden';
							document.getElementById("advertisementAmt").value = 0;
							document.getElementById("amtErrBox").innerHTML = "Amount not fixed for your selection criteria.";
							//$('#submitAdvr').prop('disabled', true);
						} else {
							document.getElementById("buttonsSaveReset").style.visibility = 'visible';
							document.getElementById("amtErrBox").innerHTML = "";
							//$('#submitAdvr').prop('disabled', false);
						}
					},
					error : function() {
						document.getElementById("advertisementAmt").value = 0;
						document.getElementById("amtErrBox").innerHTML = "Amount not fixed for your selection criteria.";
						//$('#submitAdvr').prop('disabled', true);
						document.getElementById("buttonsSaveReset").style.visibility = 'hidden';
					}
				});

		return false;
	}

	function getAmtNonHoardingAdvr() {
		var ulbid = document.getElementById("applicantUlb").value;
		var totalarea = document.getElementById("advertiseTotalArea").value;

		var subcatId = document.advAppForm.advertisementSubCategory.value;
		var catId = document.advAppForm.advertisementCategory.value;

		var lenghtInMts = document.getElementById("lengthInMtsNos").value;
		var widthInMts = document.getElementById("widthInMtsNos").value;
		var totalArea = document.getElementById("advertiseTotalArea").value;

		if (ulbid == 0 || catId == 0 || subcatId == 0 || totalarea == 0
				|| lenghtInMts == 0 || lenghtInMts == '' || widthInMts == 0
				|| widthInMts == '' || totalArea == 0 || totalArea == '') {
			document.getElementById("buttonsSaveReset").style.visibility = 'hidden';
			//$('#submitAdvr').prop('disabled', true);
			return false;
		}

		if (ulbid == 0 || catId == 0 || subcatId == 0 || totalarea == 0) {
			document.getElementById("buttonsSaveReset").style.visibility = 'hidden';
			//$('#submitAdvr').prop('disabled', true);

			return false;
		}

		var url = "${pageContext.request.contextPath}/getNonHoardingAdvrAmt.do";
		document.getElementById("advertisementAmt").value = 0;
		$
				.ajax({

					type : "POST",
					url : url,
					data : {
						i_ctgycode : catId,
						totarea : totalarea,
						i_subctgycode : subcatId,
						i_ulbobjid : ulbid
					},

					dataType : 'json',
					success : function(data) {
						document.getElementById("advertisementAmt").value = data;
						if (data == 0) {
							document.getElementById("buttonsSaveReset").style.visibility = 'hidden';
							document.getElementById("advertisementAmt").value = 0;
							document.getElementById("amtErrBox").innerHTML = "Amount not fixed for your selection criteria.";
							//$('#submitAdvr').prop('disabled', true);
						} else {
							document.getElementById("buttonsSaveReset").style.visibility = 'visible';
							document.getElementById("amtErrBox").innerHTML = "";
							//$('#submitAdvr').prop('disabled', false);
						}
					},
					error : function() {
						document.getElementById("advertisementAmt").value = 0;
						document.getElementById("amtErrBox").innerHTML = "Amount not fixed for your selection criteria.";
						//$('#submitAdvr').prop('disabled', true);
						document.getElementById("buttonsSaveReset").style.visibility = 'hidden';
					}
				});

		return false;
	}

	function getTotalArea() {

		var num=(document
		.getElementById("lengthInMtsNos").value
		* document.getElementById("widthInMtsNos").value);		
	
		document.getElementById("advertiseTotalArea").value = num.toFixed(2);
				
		if (document.getElementById("advertiseTotalArea").value > 100) {
			alert("Area cannot be greater than 100");
		}
		getAmtNonHoardingAdvr();
	}
</script>
```

2. login
```
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
<title>CDMA eTrade Application</title>
<%
	String path = request.getContextPath();
%>

<script type="text/javascript">
	//history.go(-(history.length - 1));
	window.history.forward();
	function noBack() {
		window.history.forward();
	}
	function noBack1() {
		//window.location.replace('/Tradeapplication/etradeApplication.do');
	}
</script>


</head>

<%@ taglib uri="/WEB-INF/tlds/tiles-jsp.tld" prefix="tiles"%>
<%@ taglib uri="/WEB-INF/tlds/fmt.tld" prefix="fmt"%>
<%@ taglib uri="/WEB-INF/tlds/c.tld" prefix="c"%>
<%@ taglib uri="/WEB-INF/tlds/spring.tld" prefix="spring"%>
<%@ taglib uri="/WEB-INF/tlds/spring-form.tld" prefix="form"%>
<%@ taglib uri="/WEB-INF/tlds/fn.tld" prefix="fn"%>

<script src="<%=path%>/jsp/js/jquery.min.js"></script>
<script src="<%=path%>/jsp/js/bootstrap.min.js"></script>

<link rel="stylesheet" href="<%=path%>/jsp/css/bootstrap.min.css">
<link rel="stylesheet" href="<%=path%>/jsp/css/bootstrap.3.3.7.min.css">


<body onload="noBack();" onpageshow="if (event.persisted) noBack();">



	<form method="POST" action="login.do" name="login" id="login"
		onsubmit="return checkValidation();">
		<fieldset style="position: absolute; left: 0; right: 0; margin: auto;">
			<div class="col-md-3"></div>
			<div class="col-md-6">
				<br>
				<div class="panel panel-primary">
					<div class="panel-heading">Login Here</div>
					<div class=" panel-body ">
						<div class="panel-group" align="center">

							<div class="row">
								<div class="form-group">
									<div class="inputGroupContainer">
										<div class="input-group col-md-8">
											<label class="col-md-12 control-label" for="textinput"
												style="color: black;">User Id</label>
											<div class="col-md-12">
												<input name="userId" id="userId" placeholder="User Id"
													class="form-control" type="text">
											</div>
										</div>
									</div>
								</div>

								<div class="form-group ">
									<div class="inputGroupContainer">
										<div class="input-group col-md-8">
											<label class="col-md-12 control-label" for="textinput"
												style="color: black;">Enter Password</label>

											<div class="col-md-12">
												<input name="userPassword" id="userPassword"
													placeholder="Password" class="form-control" type="password">
											</div>

										</div>
									</div>
								</div>

								<div class="col-md-12 form-group submit">
									<input type="submit" value="    Login    "
										class="btn btn-success" /> &nbsp;&nbsp;&nbsp;<input
										type="reset" value="    Reset    " class="btn btn-danger" />
								</div>
								<div class="input-group col-md-12" align="center">
									<div class="col-md-12" align="center" id="errorBox"
										style="color: red; font-size: 14px; font-weight: bold;"></div>
								</div>

							</div>
						</div>

					</div>
				</div>
			</div>
			<div class="col-md-3"></div>
		</fieldset>
	</form>
	<div id="logoutMsg">
		<span style="color: #f13611; font-size: 15px; font-weight: bold;">
			<c:out value="${message}"></c:out>
		</span>
	</div>


</body>
<link rel="stylesheet" href="<%=path%>/jsp/css/jquery-ui.css">
<script src="<%=path%>/jsp/js/jquery-1.9.1.js"></script>
<script src="<%=path%>/jsp/js/jquery-ui.js"></script>
<script type="text/javascript">
	function checkValidation() {

		var userId = document.login.userId.value;
		var userPassword = document.login.userPassword.value;
		document.getElementById("logoutMsg").innerHTML = "";
		if (userId == '' || userId == null) {
			document.getElementById("errorBox").innerHTML = "Please Enter Your Login Id";
			document.getElementById("userId").focus();
			return false;
		}
		if (userPassword == '' || userPassword == null) {
			document.getElementById("errorBox").innerHTML = "Please Enter Your Password";
			document.getElementById("userPassword").focus();
			return false;
		}

		return true;
	}
</script>

</html>
```

3. applicationStatus
```

<%-- <%@	include file="/pages/common/include.jsp"%> --%>


<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
<title>Non Hoarding Advertisement</title>
<%
	String path = request.getContextPath();
%>
</head>
<%@ taglib uri="/WEB-INF/tlds/tiles-jsp.tld" prefix="tiles"%>
<%@ taglib uri="/WEB-INF/tlds/fmt.tld" prefix="fmt"%>
<%@ taglib uri="/WEB-INF/tlds/c.tld" prefix="c"%>
<%@ taglib uri="/WEB-INF/tlds/spring.tld" prefix="spring"%>
<%@ taglib uri="/WEB-INF/tlds/spring-form.tld" prefix="form"%>
<%@ taglib uri="/WEB-INF/tlds/fn.tld" prefix="fn"%>

<script src="<%=path%>/jsp/js/jquery.min.js"></script>
<script src="<%=path%>/jsp/js/bootstrap.min.js"></script>

<link rel="stylesheet" href="<%=path%>/jsp/css/bootstrap.min.css">
<link rel="stylesheet" href="<%=path%>/jsp/css/bootstrap.3.3.7.min.css">





<script type="text/javascript">
	$(document).ready(function() {

		var fromDate = $("#startDate").datepicker({
			defaultDate : "+1w",
			dateFormat : 'mm/dd/yy',
			changeMonth : true,
			//numberOfMonths: 2,
			minDate : 0,

			onSelect : function(date) {
				var date2 = $('#startDate').datepicker('getDate');
				var datemin = $('#startDate').datepicker('getDate');
				date2.setMonth(date2.getMonth() + 11);

				$('#endDate').datepicker('option', 'minDate', datemin);

			}

		});
		var toDate = $("#endDate").datepicker({
			defaultDate : "+1w",
			minDate : 0,
			dateFormat : 'mm/dd/yy',
			changeMonth : true,
			changeYear : true,

		});

		//For Read-Only Fromdate
		$("#startDate").click(function() {
			$(this).blur();
		});
		$("#startDate").keyup(function() {
			$(this).blur();
		});

		//For Read-Only Todate 
		$("#endDate").click(function() {
			$(this).blur();
		});
		$("#endDate").keyup(function() {
			$(this).blur();
		});

	});
</script>

<form action="applicationstatus.do" method="POST" name="applstatus"
	id="applstatus" onsubmit="return checkValidation();">
	<fieldset>
	
			<div class="panel panel-primary">
				<div class="panel-heading">Application Status</div>
				<div class="panel-body">
					<div class="row">
						<div class="col-md-12">
							<div class="col-md-6">
								<div class="form-group">
									<label class="col-md-6 control-label" for="textinput">Enter
										Unique Request Number</label>
									<div class="col-md-6" style="margin-bottom: 15px;">
										<input class="form-control" name="uniqueRequestNumber"
											id="uniqueRequestNumber" placeholder="Uniq request number">
									</div>
								</div>
							</div>
							<div class="col-md-6">
								<div class="form-group">
									<div class="col-md-6" style="margin-bottom: 15px;">
										<input type="submit" class="btn btn-success"
											value="Search Application Status">

									</div>
								</div>
							</div>
						</div>
					</div>


					<c:if
						test="${applStatus.uniqueRequestNumber != 0 && not empty applStatus}">
						<div class="col-md-12">
							<br>
							<hr>
							<br>
						</div>
						<div class="col-md-12">
							<div class="col-md-6">
								<div class="form-group">
									<label class="col-md-6 control-label" for="textinput">Applicant
										Name</label>
									<div class="col-md-6" style="margin-bottom: 15px;">
										${applStatus.applicantName} ${applStatus.applicantSurName}</div>
								</div>
							</div>
							<div class="col-md-6">
								<div class="form-group">
									<label class="col-md-6 control-label" for="textinput">Uniq
										Request Number</label>
									<div class="col-md-6" style="margin-bottom: 15px;">
										${applStatus.uniqueRequestNumber}</div>
								</div>
							</div>
						</div>

						<div class="col-md-12">
							<div class="col-md-6">
								<div class="form-group">
									<label class="col-md-6 control-label" for="textinput">Application
										For</label>
									<div class="col-md-6" style="margin-bottom: 15px;">
										<c:choose>
											<c:when test="${applStatus.hoardingType=='NH'}">
												Non Hoarding Advertisement.
											</c:when>
											<c:otherwise>
												Hoarding Advertisement.
											</c:otherwise>
										</c:choose>
									</div>
								</div>
							</div>
							<div class="col-md-6">
								<div class="form-group">
									<label class="col-md-6 control-label" for="textinput">Application
										Date</label>
									<div class="col-md-6" style="margin-bottom: 15px;">
										${applStatus.entryDate}
									</div>
								</div>
							</div>

						</div>

						<div class="col-md-12">

							<div class="col-md-6">
								<div class="form-group">
									<label class="col-md-6 control-label" for="textinput">Payment
										Status</label>
									<div class="col-md-6" style="margin-bottom: 15px;">

										<c:choose>
											<c:when test="${applStatus.paymentStatus == 89}">
												<!-- 89=Y -->
												Payment Received.
											</c:when>
											<c:when test="${applStatus.paymentStatus == 78}">
												<!-- N -->										  
												 Payment Not Done.
											</c:when>
											<c:otherwise>
												<!-- 70=F -->										
										  		 Payment Failed.	
											</c:otherwise>
										</c:choose>

									</div>
								</div>
							</div>
							<div class="col-md-6">
								<div class="form-group">
									<label class="col-md-6 control-label" for="textinput">Application
										Status</label>
									<div class="col-md-6" style="margin-bottom: 15px;">

										<c:if test="${applStatus.paymentStatus == 89 }">
											<!-- 89=Y -->
											<c:choose>
												<c:when
													test="${applStatus.applicationStage=='Commissioner Rejected' || applStatus.applicationStatusFlag == 82}">
													<!-- 82=R -->
													<c:out value="${applStatus.statusComment}" />
												</c:when>
												<c:when test="${applStatus.applicationStatusFlag == 78}">
													<!-- 78=N -->										  
												  Application Under Process.
												</c:when>
												<c:when
													test="${applStatus.applicationStatusFlag == 89 && applStatus.applicationStage eq 'Commissioner Approval Completed' || applStatus.applicationStatusFlag == 89 && applStatus.applicationStage eq 'Certificate Issued'}">
													<!-- 89= Y -->									  
												   Application Approved.												  
												</c:when>
												<c:otherwise>
													<c:out value="${applStatus.applicationStage}" />
												</c:otherwise>
											</c:choose>
										</c:if>
										<c:if test="${applStatus.paymentStatus == 78}">										
										   Payment Not Done.....									
										</c:if>
										<c:if test="${applStatus.paymentStatus ==  70}">
											<!-- 70=F -->										
										   Payment Failed.....									
										</c:if>

									</div>
								</div>
							</div>
						</div>

						<div class="col-md-12">

							<div class="form-group text-center">
								<c:if
						test="${applStatus.applicationStatusFlag == 89 && applStatus.applicationStage eq 'Commissioner Approval Completed' || applStatus.applicationStatusFlag == 89 && applStatus.applicationStage eq 'Certificate Issued'}">
												
									<%-- <a
										style="text-decoration: none; font-size: 18px; font-weight: bold;"
										href="${pageContext.request.contextPath}/downloadCertificate.do?uniqueRequestNumber=<c:out value="${applStatus.uniqueRequestNumber}"/>"
										target="_blank"> Download Certificate </a> --%>
										<a href="feedback.do" target="_blank"
									style="font-size: 16px; font-weight: bold; text-decoration: none;">Feedback/Grievance</a>
								</c:if>

							
								
							
						</div>

						</div>

					</c:if>

					<div class="col-md-12 control-label" align="center"
						style="color: red; font-weight: bold; font-size: 15px;">
						<br> ${message} <br>
					</div>
				</div>

			</div>
	
	</fieldset>
</form>


<link rel="stylesheet" href="<%=path%>/jsp/css/jquery-ui.css">
<script src="<%=path%>/jsp/js/jquery-1.9.1.js"></script>
<script src="<%=path%>/jsp/js/jquery-ui.js"></script>
```










