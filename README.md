# BarcodeConversion2
--Default.aspx
<%@ Page Title="Create Indexes" Language="C#" MasterPageFile="~/Site.Master" AutoEventWireup="true" CodeBehind="Default.aspx.cs" Inherits="BarcodeConversion._Default" %>

<asp:Content ID="BodyContent" ContentPlaceHolderID="MainContent" runat="server">

	<script>
        // Hide all
	    function hideForm() {
	        //$("#formPanel").hide();
	        document.getElementById("formPanel").style.display = "none";
	    }
		// FADEOUT INDEX-SAVED MSG. FUNCTION
		function FadeOut() {
			$("table[id$='manualEntryMsg']").delay(2000).fadeOut(1000);
		}

		function FadeOut2() {
			$("table[id$='fileEntryMsg']").delay(2000).fadeOut(1000);
		}

		function FadeOut3() {
		    $.when($.ajax(hideMsg())).then(function () {
		        setTimeout(function () {
		            showMsg();
		        }, 1300);
		    });
		    function hideMsg() { $("table[id$='manualEntryMsg']").hide(); }
		    function showMsg() { $("table[id$='manualEntryMsg']").show(); }
		}

		// PRINTING INDEX SHEETS. FUNCTION
		function printing() {
		    $.when($.ajax(function1())).then(function () {
				setTimeout(function () {
				    function2();
				}, 2500);
		    });
            function function1() { window.print();}
		    function function2() { document.getElementById('<%=goToQuestion.ClientID%>').click();}
		}

	    // Manual Save Btn pause.
	    function saveBtnPause() {
	        $.when($.ajax(pauseFunction())).then(function () {
	            setTimeout(function () {
	                unpauseFunction();
	            }, 1300);
	        });
	        function pauseFunction() {
	            document.getElementById('<%=saveIndex.ClientID %>').setAttribute("disabled", "disabled");
	            document.getElementById('<%=saveAndPrint.ClientID %>').setAttribute("disabled", "disabled");
	            document.getElementById('<%=File1.ClientID %>').setAttribute("disabled", "disabled");
                document.getElementById('<%=upload.ClientID %>').setAttribute("disabled", "disabled");
	        }
	        function unpauseFunction() {
	            document.getElementById('<%=saveIndex.ClientID %>').removeAttribute("disabled");
	            document.getElementById('<%=saveAndPrint.ClientID %>').removeAttribute("disabled");
	            document.getElementById('<%=File1.ClientID %>').removeAttribute("disabled");
                document.getElementById('<%=upload.ClientID %>').removeAttribute("disabled");
	        }
	    } 
	    
		
	</script>

    <div ID="indexPage" visible="false" runat="server"></div>

    <asp:UpdatePanel runat="server">
        <Triggers>
            <asp:PostBackTrigger ControlID="upload" />
            <asp:PostBackTrigger ControlID="printIndexesBtn" />
            <asp:PostBackTrigger ControlID="saveAndPrint" />
        </Triggers>
        <ContentTemplate>
	        <div ID="formPanel" runat="server">
		        <asp:Panel ID="satisfied" Visible="false" style="margin-top:150px;margin-bottom:200px;" runat="server" Height="120"> 
			        <asp:Panel style="margin:0px auto;padding:20px;width:600px;" runat="server">
				        <table class="dataSection" style="font-size:12px;margin:10px 0 0 -100px;">
					        <tr >
						        <td style="text-align:left;">
							        <h5 style="display:inline;"><asp:Label ID="sortOrder" style="padding-top:25px;" Text="Satisfied with the Index Sheet(s)?" runat="server"></asp:Label></h5>
							        <asp:LinkButton ID="yesBtn" CssClass="btn btn-primary" style="margin-left:10px;" runat="server" OnClick="satisfied_Click">Yes</asp:LinkButton>
							        <asp:LinkButton ID="noBtn" CssClass="btn btn-danger" style="margin-left:10px;" runat="server" OnClick="satisfied_Click">No</asp:LinkButton>
						        </td>
					        </tr> 
					        <tr><td style="text-align:left;font-size:14px;padding-top:10px;color:#595959;">Click YES if you did print and are satisfied with the Index Sheets. </td></tr>
                            <tr><td style="text-align:left;font-size:14px;padding-top:2px;color:#595959;">Click NO if you did not print or are not satisfied with the Index Sheets.</td></tr>                
				        </table>
			        </asp:Panel>
		        </asp:Panel>

		        <asp:Panel ID="formPanelJobSelection" runat="server">
			        <div style="margin-top:45px; margin-bottom:40px; height:50px; border-bottom:solid 1px green;width:899px;">
				        <h3 style="margin-top:45px;color:#595959;">Index Setup</h3>
			        </div>
			        <asp:Button ID="selectJobBtn" Visible="false" runat="server" Text="Generate Jobs" onclick="selectJob_Click" />

			        <table class = table>
				        <tr> <th colspan="2" style="font-family:Arial;font-size:14px;">Select Job: </th></tr>
				        <tr style="vertical-align:central;">
					        <td style="width: 186px;"><asp:Label ID="selectJobLabel" Font-Size="11" runat="server">Job Abbreviation:</asp:Label></td>
					        <td> 
						        <asp:DropDownList  ID="selectJob" OnSelectedIndexChanged="onJobSelect" onmousedown="this.focus()" AutoPostBack="true" runat="server">
							        <asp:ListItem Value="Select">Select </asp:ListItem>
						        </asp:DropDownList>
					        </td>
				        </tr>
			        </table> 
			        <asp:Panel ID="noJobsFound" Visible="false" runat="server"><h3> You currently have no assigned jobs.</h3> </asp:Panel>
		        </asp:Panel>

		        <asp:Panel ID="indexCreationUpdatePanel" runat="server" >
                    <asp:Table ID="notConfigScreenMsg" style="margin-top:10px;" runat="server"></asp:Table>
                    <asp:Panel ID="indexCreationSection" Visible="false" style="width:auto; margin-top:20px;" runat="server">
			            <asp:Label colspan="2" style="font-family:Arial;font-weight:bold;display:block;" runat="server">&nbsp;Upload Index Data File: </asp:Label>
			            <asp:Panel CssClass="card" runat="server" style="background-color:aliceblue;margin-top:5px;">
				            <table style="width:470px;" >
					            <tr style="height:40px;margin-top:10px;">
						            <td style="width:315px; padding-left:5px;">
						                <asp:FileUpload id="File1" style="width:300px;height:30px;font-size:14px;" type=file name=File1 runat="server" /></td>
						            <td style="text-align:right; font-size:13px;padding-right:7px;">
							            <asp:linkButton ID="upload" CssClass="btn btn-primary" OnClick="upload_Click" runat="server">
							            <i class="fa fa-upload" aria-hidden="true"></i> Upload</asp:linkButton></td>
					            </tr>
				            </table>  
			            </asp:Panel>

			            <asp:Panel id="uploadedFileMenu" Visible="false" style="display:block;width:502px;margin-top:10px;" runat="server">
				            <table style="padding-top:5px;" runat="server">
					            <tr>
						            <td style="padding-right:15px;width:330px;"><asp:Label ID ="uploadSuccess" Text="File Uploaded successfully!" runat="server"></asp:Label></td>
						            <td style="text-align:right; padding-right:5px;">
							            <asp:Button ID="viewContentBtn" CssClass="btn btn-primary" style="padding:0px 6px 1px 6px;" Text="View" Font-Size="8" OnClick="viewContent_Click" runat="server"/></td>
						            <td style="text-align:right; padding-right:5px;">
							            <asp:Button ID="saveIndexesBtn" CssClass="btn btn-primary" style="padding:0px 6px 1px 6px;" Text="Save" Font-Size="8" OnClientClick="return confirm('CORRECT FILE UPLOADED?\n\nMake sure the uploaded file corresponds to the currently selected Job.\nWish to proceed and save indexes?');" OnClick="saveIndexes_Click" runat="server"/></td>
						            <td style="text-align:right;">
							            <asp:Button ID="printIndexesBtn" CssClass="btn btn-primary" style="padding:0px 6px 1px 6px;" Text="Save & Print" Font-Size="8" OnClientClick="return confirm('CORRECT FILE UPLOADED?\n\nMake sure the uploaded file corresponds to the currently selected Job.\nWish to proceed and print barcodes?');" OnClick="printIndexes_Click" runat="server"/></td>
					            </tr>
					            <tr>
						            <td style="padding-right:15px;"><asp:Label ID ="uploadHidden" Text="" Visible="false" runat="server"></asp:Label></td>
					            </tr>
				            </table>

				            <asp:GridView ID="GridView1" CssClass="mGrid" Font-Size="10" OnRowDataBound="rowDataBound" style="margin-top:20px;" Width="100%" runat="server"></asp:GridView>
			            </asp:Panel>

			            <div id="fileMsgDiv" style="margin:10px 0 20px 0; height:30px;" runat="server"><asp:table id="fileEntryMsg" runat="server"></asp:table></div>
				  
			            <h3 style="margin-top:20px;color:#666666;">Index Creation</h3>

			            <table id="jobControls" class = table runat="server">
				            <tr> <th colspan="3" style="font-family:Arial;font-size:14px;">Index Data Information: </th></tr>
				            <tr>
					            <td style="vertical-align:middle;width:185px;"><asp:Label ID="LABEL1" Font-Size="11" Text="LABEL1" Visible="false" runat="server"></asp:Label></td>
					            <td>
						            <asp:TextBox ID="label1Box" Visible="false" placeholder=" Required" onfocus="this.select()" runat="server"></asp:TextBox>
						            <asp:DropDownList ID="label1Dropdown" onmousedown="this.focus()" Visible="false" runat="server">
							            <asp:ListItem Value=""></asp:ListItem>
						            </asp:DropDownList>
					            </td>
				            </tr>
				            <tr>
					            <td style="vertical-align:middle;"><asp:Label ID="LABEL2" Font-Size="11" Text="LABEL2"  Visible="false" runat="server"></asp:Label></td>
					            <td>
						            <asp:TextBox ID="label2Box" Visible="false" onfocus="this.select()" runat="server"></asp:TextBox>
						            <asp:DropDownList ID="label2Dropdown" onmousedown="this.focus()" Visible="false" runat="server">
							            <asp:ListItem Value=""></asp:ListItem>
						            </asp:DropDownList>
					            </td>
				            </tr>
				            <tr>
					            <td style="vertical-align:middle;"><asp:Label ID="LABEL3" Font-Size="11" Text="LABEL3"  Visible="false" runat="server"></asp:Label></td>
					            <td>
						            <asp:TextBox ID="label3Box" Visible="false" onfocus="this.select()" runat="server"></asp:TextBox>
						            <asp:DropDownList ID="label3Dropdown" onmousedown="this.focus()" Visible="false" runat="server">
							            <asp:ListItem Value=""></asp:ListItem>
						            </asp:DropDownList>
					            </td>
				            </tr>
				            <tr>
					            <td style="vertical-align:middle;"><asp:Label ID="LABEL4" Font-Size="11" Text="LABEL4"  Visible="false" runat="server"></asp:Label></td>
					            <td>
						            <asp:TextBox ID="label4Box" Visible="false" onfocus="this.select()" runat="server"></asp:TextBox>
						            <asp:DropDownList ID="label4Dropdown" onmousedown="this.focus()" Visible="false" runat="server">
							            <asp:ListItem Value=""></asp:ListItem>
						            </asp:DropDownList>
					            </td>
				            </tr>
				            <tr>
					            <td style="vertical-align:middle;"><asp:Label ID="LABEL5" Font-Size="11" Text="LABEL5"  Visible="false" runat="server"></asp:Label></td>
					            <td>
						            <asp:TextBox ID="label5Box" Visible="false" onfocus="this.select()" runat="server"></asp:TextBox>
						            <asp:DropDownList ID="label5Dropdown" onmousedown="this.focus()" Visible="false" runat="server">
							            <asp:ListItem Value=""></asp:ListItem>
						            </asp:DropDownList>
					            </td>
				            </tr>
                            <tr>
					            <td style="vertical-align:middle;"><asp:Label ID="LABEL6" Font-Size="11" Text="LABEL5"  Visible="false" runat="server"></asp:Label></td>
					            <td>
						            <asp:TextBox ID="label6Box" Visible="false" onfocus="this.select()" runat="server"></asp:TextBox>
						            <asp:DropDownList ID="label6Dropdown" onmousedown="this.focus()" Visible="false" runat="server">
							            <asp:ListItem Value=""></asp:ListItem>
						            </asp:DropDownList>
					            </td>
				            </tr>
                            <tr>
					            <td style="vertical-align:middle;"><asp:Label ID="LABEL7" Font-Size="11" Text="LABEL5"  Visible="false" runat="server"></asp:Label></td>
					            <td>
						            <asp:TextBox ID="label7Box" Visible="false" onfocus="this.select()" runat="server"></asp:TextBox>
						            <asp:DropDownList ID="label7Dropdown" onmousedown="this.focus()" Visible="false" runat="server">
							            <asp:ListItem Value=""></asp:ListItem>
						            </asp:DropDownList>
					            </td>
				            </tr>
                            <tr>
					            <td style="vertical-align:middle;"><asp:Label ID="LABEL8" Font-Size="11" Text="LABEL5"  Visible="false" runat="server"></asp:Label></td>
					            <td>
						            <asp:TextBox ID="label8Box" Visible="false" onfocus="this.select()" runat="server"></asp:TextBox>
						            <asp:DropDownList ID="label8Dropdown" onmousedown="this.focus()" Visible="false" runat="server">
							            <asp:ListItem Value=""></asp:ListItem>
						            </asp:DropDownList>
					            </td>
				            </tr>
                            <tr>
					            <td style="vertical-align:middle;"><asp:Label ID="LABEL9" Font-Size="11" Text="LABEL5"  Visible="false" runat="server"></asp:Label></td>
					            <td>
						            <asp:TextBox ID="label9Box" Visible="false" onfocus="this.select()" runat="server"></asp:TextBox>
						            <asp:DropDownList ID="label9Dropdown" onmousedown="this.focus()" Visible="false" runat="server">
							            <asp:ListItem Value=""></asp:ListItem>
						            </asp:DropDownList>
					            </td>
				            </tr>
                            <tr>
					            <td style="vertical-align:middle;"><asp:Label ID="LABEL10" Font-Size="11" Text="LABEL5"  Visible="false" runat="server"></asp:Label></td>
					            <td>
						            <asp:TextBox ID="label10Box" Visible="false" onfocus="this.select()" runat="server"></asp:TextBox>
						            <asp:DropDownList ID="label10Dropdown" onmousedown="this.focus()" Visible="false" runat="server">
							            <asp:ListItem Value=""></asp:ListItem>
						            </asp:DropDownList>
					            </td>
				            </tr>
                            <tr>
                                <td colspan="2"  style="padding-top:15px;">
                                    <asp:CheckBox ID="lastValuesEntered" runat="server"/>
                                    <asp:Label style="margin-left:10px;" Font-Size="11" runat="server">Remember last value(s) entered.</asp:Label></td>
                            </tr>
			            </table>

	                        <%--
		                    Not utilized (yet).
		                    <p>
			                    Barcode thickness:
			                    <asp:DropDownList ID="ddlBarcodeThickness" runat="server">
				                    <asp:ListItem Value="1">Thin</asp:ListItem>
				                    <asp:ListItem Value="2">Medium</asp:ListItem>
				                    <asp:ListItem Value="3">Thick</asp:ListItem>    
			                    </asp:DropDownList>
		                    </p>
			                        <%-- Msgs showing up when successful save or saveAndPrint operations happens.--%>
			            <div style="margin-top:25px;height:25px;"><asp:table id="manualEntryMsg" runat="server"></asp:table></div>

			            <asp:Panel ID="generateIndexSection" CssClass="card" style="background-color:#e6f3ff;" Visible="false" runat="server">
				            <table class = tableFull style=" width:470px;">
					            <tr >
						            <td style="text-align:left;">
							            <asp:linkButton ID="saveIndex" CssClass="btn btn-primary" runat="server" Font-Size="10" OnClick="saveIndex_Click">Save Index</asp:linkButton></td>
						            <td style="text-align:right; padding-right:5px;">
							            <asp:Button ID="saveAndPrint" CssClass="btn btn-primary" runat="server" Text="Save & Print" Font-Size="10" OnClick="printIndexes_Click" /></td>
					            </tr>               
				            </table>
			            </asp:Panel>
                    </asp:Panel>
		        </asp:Panel>

		        <div style="margin-top:40px;"></div>

		        <%--Link to Print Indexes page --%>
                <div id="linkToUnprinted" runat="server">  
		            <span  style="font-size:medium;" runat="server">View your unprinted indexes</span>   
		            <asp:HyperLink ID="HyperLink1" Font-Underline="false" runat="server" NavigateUrl="~/Indexes">
			            <span style="font-size:medium;"> here.</span>
		            </asp:HyperLink>
                </div>
                <%--Some hidden fields --%>
		        <input id="indexString" type="hidden"  runat="server"  value=""/>
	        </div>
	   
        </ContentTemplate>
    </asp:UpdatePanel>

    <%-- Questions --%>
	<div style="display:none;">
		<asp:Button ID="goToQuestion" runat="server" onclick="goToQuestion_Click"/>
	</div>

</asp:Content>
--Default.aspx.cs
using System;
using System.Collections.Generic;
using System.Web.UI;
using System.Web.UI.WebControls;
using System.Data.SqlClient;
using System.Globalization;
using BarcodeConversion.App_Code;
using System.Text.RegularExpressions;
using Microsoft.VisualBasic.FileIO;
using System.IO;
using System.Linq;
using System.Data;
using System.Web;

namespace BarcodeConversion
{

    public partial class _Default : Page
    {
        public void Page_Init(object o, EventArgs e)
        {
            Page.MaintainScrollPositionOnPostBack = true;
            
        }

        protected void Page_Load(object sender, EventArgs e)
        {
            if (!Page.IsPostBack)
            {
                label1Box.Focus();
                Page.Form.Attributes.Add("enctype", "multipart/form-data");
                // 'JOB ABBREVIATION' DROPDOWN FILL: POPULATE DROPDOWN
                selectJob_Click(new object(), new EventArgs());
            }
            setDropdownColor();
        }


        // 'JOB ABBREVIATION' DROPDOWN ITEM SELECTION: SET & DISPLAY CONTROLS OF SELECTED JOB. 
        protected void onJobSelect(object sender, EventArgs e)
        {
            try
            {
                // Set stage
                ViewState["manualEntries"] = null;
                ViewState["fileContent"] = null;
                generateIndexSection.Visible = false;
                indexCreationSection.Visible = false;
                uploadSuccess.Text = "";
                viewContentBtn.Text = "View";
                viewContentBtn.Visible = false;
                saveIndexesBtn.Visible = false;
                printIndexesBtn.Visible = false;
                GridView1.Visible = false;
                uploadedFileMenu.Visible = false;

                for (int i = 1; i <= 10; i++)
                {
                    Label l = this.Master.FindControl("MainContent").FindControl("LABEL" + i) as Label;
                    l.Visible = false;
                    TextBox t = this.Master.FindControl("MainContent").FindControl("label" + i + "Box") as TextBox;
                    t.Visible = false;
                    t.Text = string.Empty;
                    DropDownList d = this.Master.FindControl("MainContent").FindControl("label" + i + "Dropdown") as DropDownList;
                    d.Visible = false;
                }

                // Make sure a job is selected
                if (this.selectJob.SelectedValue != "Select")
                {
                    // First, get selected job ID.
                    int jobID = getJobId(this.selectJob.SelectedValue);

                    if (jobID <= 0)
                    {
                        string msg = "Error 02:     Selected job not found. Contact system admin.";
                        ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                        selectJob.SelectedValue = "Select";
                        return;
                    }

                    // Then, check whether that job is configured, if so, display controls.
                    using (SqlConnection con = Helper.ConnectionObj)
                    {
                        using (SqlCommand cmd = con.CreateCommand())
                        {
                            cmd.CommandText = "SELECT JOB_ID, LABEL1, LABEL2, LABEL3, LABEL4, LABEL5, LABEL6, LABEL7, LABEL8, LABEL9, LABEL10, " +
                                                             "REGEX1, REGEX2, REGEX3, REGEX4, REGEX5, REGEX6, REGEX7, REGEX8, REGEX9, REGEX10, " +
                                                             "ALERT1, ALERT2, ALERT3, ALERT4, ALERT5, ALERT6, ALERT7, ALERT8, ALERT9, ALERT10, " +
                                                             "TABLEID1, TABLEID2, TABLEID3, TABLEID4, TABLEID5, TABLEID6, TABLEID7, TABLEID8, TABLEID9, TABLEID10 " +
                                              "FROM JOB_CONFIG_INDEX " + 
                                              "WHERE JOB_ID=@jobID";
                            cmd.Parameters.AddWithValue("@jobID", jobID);
                            con.Open();
                            using (SqlDataReader reader = cmd.ExecuteReader())
                            {
                                if (reader.HasRows)
                                {
                                    indexCreationSection.Visible = true;
                                    int i = 1;
                                    int j = 1;
                                    var regexList = new List<Tuple<string, string, string>> { };

                                    // Set & display controls
                                    if (reader.Read())
                                    {
                                        while (i <= 10)
                                        {
                                            if (reader.GetValue(i) != DBNull.Value) // If label i is set
                                            {
                                                if (reader.GetValue(i + 30) == DBNull.Value) // If control is a Textbox
                                                {
                                                    var tuple = Tuple.Create("", "", "");
                                                    string label = (string)reader.GetValue(i);
                                                    string regex = string.Empty;
                                                    string alert = string.Empty;
                                                    if (reader.GetValue(i + 10) != DBNull.Value) // if regex i is set
                                                    {
                                                        regex = (string)reader.GetValue(i + 10);
                                                        alert = (string)reader.GetValue(i + 20);
                                                    }
                                                    tuple = Tuple.Create(label, regex, alert);

                                                    regexList.Add(tuple);
                                                    string text = label;
                                                    //text = CultureInfo.CurrentCulture.TextInfo.ToTitleCase(text.ToLower());
                                                    Label l = this.Master.FindControl("MainContent").FindControl("LABEL" + j) as Label;
                                                    l.Text = text + " :";
                                                    l.Visible = true;
                                                    TextBox t = this.Master.FindControl("MainContent").FindControl("label" + j + "Box") as TextBox;
                                                    t.Visible = true;

                                                    if (i == 1)
                                                    {
                                                        t.Attributes["placeholder"] = "Required";
                                                        t.Focus();
                                                    }
                                                    else
                                                    {
                                                        if (regex == string.Empty) t.Attributes["placeholder"] = "Optional";
                                                        else t.Attributes["placeholder"] = "Required";
                                                    }
                                                    //j += 1;
                                                }
                                                else    // If control is a dropdown
                                                {       
                                                    string label = (string)reader.GetValue(i);
                                                    var tuple = Tuple.Create(label, "", "");
                                                    regexList.Add(tuple);
                                                    //label = CultureInfo.CurrentCulture.TextInfo.ToTitleCase(label.ToLower());
                                                    Label l = this.Master.FindControl("MainContent").FindControl("LABEL" + j) as Label;
                                                    l.Text = label + " :";
                                                    l.Visible = true;
                                                    DropDownList d = this.Master.FindControl("MainContent").FindControl("label" + j + "Dropdown") as DropDownList;
                                                    d.Visible = true;
                                                    d.Items.Clear(); // Clear current dropdown items
                                                    d.Items.Add("");

                                                    // Fill dropdown
                                                    using (SqlCommand cmd2 = con.CreateCommand())
                                                    {
                                                        cmd2.CommandText = "SELECT VALUE FROM INDEX_TABLE_FIELD WHERE ID=@id ORDER BY ORD";
                                                        cmd2.Parameters.AddWithValue("@id", reader.GetValue(i + 30).ToString());
                                                        using (SqlDataReader reader2 = cmd2.ExecuteReader())
                                                        {
                                                            while (reader2.Read())
                                                            {
                                                                d.Items.Add(reader2.GetValue(0).ToString());
                                                            }
                                                        }
                                                    }
                                                }

                                            }
                                            else
                                            {
                                                var tuple = Tuple.Create("", "", "");
                                                regexList.Add(tuple);
                                            } 
                                            i += 1; j += 1; 
                                        }
                                        Session["regexList"] = regexList; // Contains (label,regex,alert) for each label set at Index Config Section
                                    }
                                    generateIndexSection.Visible = true;
                                    lastValuesEntered.Checked = false;
                                }
                                else
                                {
                                    string msg = "The \"" + selectJob.SelectedValue + "\" job that you selected has not yet been configured."
                                                + " Only jobs listed in green can be processed.";
                                    var screenMsg = new TableCell();
                                    var screenMsgRow = new TableRow();
                                    screenMsg.Text = msg;
                                    screenMsg.Attributes["style"] = "color:#ff3333;";
                                    screenMsgRow.Cells.Add(screenMsg);
                                    notConfigScreenMsg.Rows.Add(screenMsgRow);
                                    selectJob.SelectedValue = "Select";
                                    return;
                                }
                            }
                        }
                    }
                }
            }
            catch (Exception ex)
            {
                string msg = "Issue occured while attempting to retrieve selected job index data controls. Contact system admin.";
                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('Error 03: " + msg + "');", true);
                // Log the exception and notify system operators
                ExceptionUtility.LogException(ex);
                ExceptionUtility.NotifySystemOps(ex);
            }
        }



        // 'UPLOAD' CLICKED: UPLOAD CSV INDEX DATA FILE.
        protected void upload_Click(object sender, EventArgs e)
        {
            if ((File1.PostedFile != null) && (File1.PostedFile.ContentLength > 0))
            {
                try
                {
                    // Clear previous upload displays
                    uploadSuccess.Text = "";
                    viewContentBtn.Text = "View";
                    viewContentBtn.Visible = false;
                    saveIndexesBtn.Visible = false;
                    printIndexesBtn.Visible = false;
                    GridView1.Visible = false;

                    // Check file extension first
                    String extension = Path.GetExtension(File1.PostedFile.FileName);
                    if (extension.ToLower() != ".csv")
                    {
                        string msg = "Only csv files are allowed.";
                        string color = "#ff3333;";
                        onScreenMsg(msg, color, "file");
                        return;
                    }

                    // Upload file
                    string fileName = Path.GetFileName(File1.PostedFile.FileName);
                    string SaveLocation = Server.MapPath("App_Data") + "\\" + fileName;

                    // Save file
                    if (File.Exists(SaveLocation)) File.Delete(SaveLocation);
                    File1.PostedFile.SaveAs(SaveLocation);

                    bool isFileValid = true;
                    bool isFileBlank = true;
                    var fileContent = new List<List<string>>();

                    // Process file for error
                    using (TextFieldParser parser = new TextFieldParser(SaveLocation))
                    {
                        parser.TextFieldType = FieldType.Delimited;
                        parser.SetDelimiters(",");
                        while (!parser.EndOfData)
                        {
                            isFileBlank = false;

                            // Process row
                            var lineNumber = parser.LineNumber;
                            string[] fields = parser.ReadFields();
                            List<string> line = new List<string>(fields);

                            // Check if row is made of blank entries
                            bool noEntry = true;
                            foreach (var item in line)
                                if (item != string.Empty) noEntry = false;
                            if (noEntry == true)
                            {
                                string msg = "Row " +lineNumber+ ": All row fields can not be blank. At least one index data must be present.";
                                string color = "#ff3333;";
                                onScreenMsg(msg, color, "file");
                                return;
                            }
                            
                            fileContent.Add(line);

                            var regexList = (List<Tuple<string, string, string>>)Session["regexList"];
                            int labelsCount = 0;
                            int regexCount = 0;
                            int counter = 0;
                            bool firstLabelRegexSet = false;
                            string required = string.Empty;
                            foreach (Tuple<string,string,string>controlSettings in regexList)
                            {
                                counter++;
                                if (controlSettings.Item1 != string.Empty) labelsCount++; // Check how many controls were set
                                if (controlSettings.Item2 != string.Empty)  // Check how many regex were set
                                {
                                    regexCount++;
                                    if (counter == 1) firstLabelRegexSet = true;
                                }  
                            }

                            // If less row items than the min required
                            if (line.Count < regexCount)
                            {
                                int requiredFields;
                                if (firstLabelRegexSet) requiredFields = regexCount;
                                else requiredFields = regexCount + 1;
                                string msg = requiredFields + " of the " + labelsCount + " fields for this job are required fields. Row " + lineNumber + " in your csv file seems to have less fields than required.";
                                string color = "#ff3333;";
                                onScreenMsg(msg, color, "file");

                                // Clear list of file contents if errors found
                                fileContent.Clear();
                                return;
                            }

                            // If more row items than the max required
                            if (line.Count > labelsCount)
                            {
                                string msg = "This job requires no more than " + labelsCount + " column(s) in the csv file. A row with " + line.Count + " fields was detected.";
                                string color = "#ff3333;";
                                onScreenMsg(msg, color, "file");
                                isFileValid = false;

                                // Clear list of file contents if errors found
                                fileContent.Clear();
                                return;
                            }

                            // Then, check whether any line entry is a dropdown value.
                            int jobID = getJobId(this.selectJob.SelectedValue);
                            using (SqlConnection con = Helper.ConnectionObj)
                            {
                                using (SqlCommand cmd = con.CreateCommand()) // Retrieve all the tableids
                                {
                                    cmd.CommandText = "SELECT TABLEID1, TABLEID2, TABLEID3, TABLEID4, TABLEID5, TABLEID6, TABLEID7, TABLEID8, TABLEID9, TABLEID10 " +
                                                      "FROM JOB_CONFIG_INDEX " +
                                                      "WHERE JOB_ID=@jobID";
                                    cmd.Parameters.AddWithValue("@jobID", jobID);
                                    con.Open();
                                    using (SqlDataReader reader = cmd.ExecuteReader())
                                    {
                                        if (reader.HasRows && reader.Read())
                                        {
                                            for (int i = 0; i <= 9; i++) // For each tableid
                                            {
                                                bool found = false;
                                                string validEntries = string.Empty;
                                                if (reader.GetValue(i).ToString() != DBNull.Value.ToString()) // If tableid[i] exists
                                                {
                                                    using (SqlCommand cmd2 = con.CreateCommand()) // Retrieve all values related to the tableid
                                                    {
                                                        cmd2.CommandText = "SELECT VALUE FROM INDEX_TABLE_FIELD WHERE ID=@id ORDER BY ORD";
                                                        cmd2.Parameters.AddWithValue("@id", reader.GetValue(i).ToString());
                                                        using (SqlDataReader reader2 = cmd2.ExecuteReader()) // Get tableid values
                                                        {
                                                            while (reader2.Read())  // For each tableid value, check whether line entry is among those values
                                                            {
                                                                string dropdownValue = reader2.GetValue(0).ToString();
                                                                if ((String.Compare(line[i],dropdownValue,true) == 0) || line[i] == string.Empty)
                                                                    found = true;
                                                                validEntries += dropdownValue + ", ";
                                                            }
                                                        }
                                                    }

                                                    if (found == false) // if line entry not among tableid values
                                                    {
                                                        uploadSuccess.Visible = false;
                                                        string msg;
                                                        if (line[i] != string.Empty)
                                                            msg = "Value \"" + line[i] + "\" at row " + lineNumber + " of column " + (i + 1) + 
                                                            " is invalid. Valid entry must be one of following: " + validEntries.Remove(validEntries.Length - 2, 2) + ", or blank."; 
                                                        else
                                                            msg = "Value at row " + lineNumber + " of column " + (i + 1) + " does not exist. Valid entry must be one of following: " 
                                                            + validEntries.Remove(validEntries.Length - 2, 2) + ", or blank.";
                                                        string color = "#ff3333;";
                                                        onScreenMsg(msg, color, "file");
                                                        return;
                                                    }
                                                }
                                            }
                                        }
                                    }
                                }
                            }

                            //Process field for regex
                            for (int i = 1; i <= regexList.Count; i++)
                            {
                                if (regexList[i - 1].Item2 != string.Empty) // If regex exists for this label
                                {
                                    string label = regexList[i - 1].Item1;
                                    string pattern = @regexList[i - 1].Item2;
                                    string entry = fields[i - 1];
                                    Regex r = new Regex(pattern, RegexOptions.IgnoreCase);
                                    
                                    if ((i - 1) > fields.Length)
                                    {
                                        string msg1 = label + ": " + regexList[i - 1].Item3;
                                        string msg = "Line " + lineNumber + "is missing required item " + i;
                                        string color = "#ff3333;";
                                        onScreenMsg(msg, color, "file");
                                        isFileValid = false;

                                        return;
                                    }

                                    if (!r.IsMatch(fields[i - 1]))
                                    {
                                        string msg1 =  "Invalid value(s) in column " + i + ":  " + regexList[i - 1].Item3;
                                        string color = "#ff3333;";
                                        onScreenMsg(msg1, color, "file");

                                        string value, msg;
                                        if (fields[i - 1] == string.Empty)
                                        {
                                            value = "blank";
                                            msg = "For instance: Value in row " + lineNumber + " of column " + i + " was left blank."; 
                                        }
                                        else { 
                                            value = fields[i - 1];
                                            msg = "For instance: Value \"" + (fields[i - 1]) + "\" in row " + lineNumber + " of column " + i + " is not valid.";
                                        }
                                        onScreenMsg(msg, color, "file");
                                        isFileValid = false;
                                        return;
                                    }
                                }
                                else
                                {
                                    TextBox c = this.Master.FindControl("MainContent").FindControl("label" + i + "Box") as TextBox;
                                    DropDownList d = this.Master.FindControl("MainContent").FindControl("label" + i + "Dropdown") as DropDownList;
                                    if (c.Visible == true)
                                    {
                                        if (i == 1 && fields[0] == string.Empty)
                                        {
                                            string msg = "Fields in the first column cannot be blank.";
                                            string msg2 = "For instance: Item " + lineNumber + " in the first column is blank.";
                                            string color = "#ff3333;";
                                            onScreenMsg(msg, color, "file");
                                            onScreenMsg(msg2, color, "file");
                                            isFileValid = false;
                                            return;
                                        }
                                    }
                                }
                            }
                        }
                    }

                    // Check if File is blank
                    if (isFileBlank == true)
                    {
                        string msg = "The csv file seems to have no data.";
                        string color = "#ff3333;";
                        onScreenMsg(msg, color, "file");
                        return;
                    }

                    // Process file for Barcode indexing if no error found
                    if (isFileValid)
                    {
                        string shortFileName;
                        if (fileName.Length > 17)
                            shortFileName = fileName.Substring(0, 9) + " ... " + fileName.Substring((fileName.Length - 6), 6);
                        else shortFileName = fileName;
                        uploadSuccess.Text = "\"" + shortFileName + "\"" + " file uploaded successfully!";
                        uploadHidden.Text = fileName;
                        uploadSuccess.Attributes["style"] = "color:green;";
                        uploadedFileMenu.Visible = true;
                        uploadSuccess.Visible = true;
                        viewContentBtn.Visible = true;
                        saveIndexesBtn.Visible = true;
                        printIndexesBtn.Visible = true;
                    }
                    else
                    {
                        // Delete file if errors found
                        File.Delete(SaveLocation);
                        fileContent.Clear();
                    }
                }
                catch (Exception ex)
                {
                    string msg = "Error 4: Could not upload file. Check that it is a csv file and that its structure & contents belong to the currently selected Job.";
                    ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);

                    // Log the exception and notify system operators
                    ExceptionUtility.LogException(ex);
                    ExceptionUtility.NotifySystemOps(ex);
                    return;
                }
            }
            else
            {
                string msg = "Please select a csv file to upload.";
                string color = "#ff3333;display:block;";
                onScreenMsg(msg, color, "file");
            }
        }



        // FILE 'SAVE' CLICKED: SAVE UPLOADED FILE CONTENTS
        protected void saveIndexes_Click(object sender, EventArgs e)
        {
            try
            {
                ViewState["manualEntries"] = null; // Clearing any cached manual entries
                ViewState["manualPrintCancelled"] = null; 

                // Get file name & Set save location
                var fileName = uploadHidden.Text;
                if (fileName == string.Empty)
                {
                    string msg = "Error 4a:  Could not retrieve file name. Contact system admin. ";
                    ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                    return;
                }
                string SaveLocation = Server.MapPath("App_Data") + "\\" + fileName;
                var fileContent = new List<List<string>>();
                int countSavedRecords = 0;
                int countFileRecords = File.ReadLines(SaveLocation).Count();

                // Process file to generate & save indexes
                using (TextFieldParser parser = new TextFieldParser(SaveLocation))
                {
                    parser.TextFieldType = FieldType.Delimited;
                    parser.SetDelimiters(",");
                    while (!parser.EndOfData)
                    {
                        // Get row entries
                        var lineNumber = parser.LineNumber;
                        string[] fields = parser.ReadFields();
                        List<string> lineEntries = new List<string>(fields);

                        // Get number of controls set for this job
                        var regexList = (List<Tuple<string, string, string>>)Session["regexList"];
                        int labelsCount = 0;
                        foreach (Tuple<string, string, string> controlSettings in regexList)
                        {
                            if (controlSettings.Item1 != string.Empty)
                                labelsCount++;
                        }
                        
                        var newLineEntries = new List<string>();
                        if (lineEntries.Count <= labelsCount)
                        {
                            foreach (string field in lineEntries)
                            {
                                newLineEntries.Add(field);
                            }
                            int diff = labelsCount - lineEntries.Count;
                            for (int i = 1; i <= diff; i++) newLineEntries.Add(string.Empty);
                            lineEntries.Clear();
                            lineEntries = newLineEntries;
                        }

                        // add blanks to make 10 items per row
                        if (lineEntries.Count < 10)
                        {
                            int diff = 10 - lineEntries.Count;
                            for (int i = 1; i <= diff; i++) lineEntries.Add(string.Empty);
                        }
                        // Check to make sure that file's line entries are within dropdown range if it applies
                        // First, get selected job ID.
                        int jobID = getJobId(this.selectJob.SelectedValue);

                        if (jobID <= 0)
                        {
                            string msg = "Error 4b: Selected job not found. Contact system admin.";
                            ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                            selectJob.SelectedValue = "Select";
                            return;
                        }

                        // Get barcode
                        string barcodeIndex = generateBarcode();
                        if (barcodeIndex.Contains("Error"))
                        {
                            string msg = "Error 4c:  Error occurred while generating barcode! Contact system admin.";
                            ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                            return;
                        }
                        // Save barcode and entries
                        string result = saveIndexAndEntries(barcodeIndex, lineEntries);
                        if (result.Contains("Error"))
                        {
                            string msg = "Only " + countSavedRecords + " records were saved! Contact system admin.";
                            string color = "#ff3333;";
                            onScreenMsg(msg, color, "file");
                            return;
                        }
                        // Add barcode to the end of entries
                        lineEntries.Add(barcodeIndex);
                        fileContent.Add(lineEntries);
                        countSavedRecords++;
                    }
                }

                // Make sure all file records have been saved
                if (countFileRecords >= countSavedRecords)
                {
                    uploadSuccess.Visible = false;
                    string msg = countSavedRecords + " index string(s) saved.";
                    string color = "green;";
                    onScreenMsg(msg, color, "file");

                    uploadedFileMenu.Visible = false;
                    viewContentBtn.Visible = false;
                    saveIndexesBtn.Visible = false;
                    printIndexesBtn.Visible = false;
                    GridView1.Visible = false;

                    ViewState["fileContent"] = fileContent;
                    ScriptManager.RegisterStartupScript(indexCreationUpdatePanel, indexCreationUpdatePanel.GetType(), "fadeoutOp", "FadeOut2();", true);
                }
                ViewState["filePrintCancelled"] = countSavedRecords;
            }
            catch(Exception ex)
            {
                string msg = "Error 4d:  Couldn't process file. Contact system admin.";
                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);

                // Log the exception and notify system operators
                ExceptionUtility.LogException(ex);
                ExceptionUtility.NotifySystemOps(ex);
                return;
            }
        }



        // 'SAVE & PRINT' CLICKED: SAVE & PRINT UPLOADED FILE CONTENTS or Manual entries
        protected void printIndexes_Click(object sender, EventArgs e)
        {
            try
            {
                // Set stage
                Button b = (Button)sender;

                // First, save index(es) if File Entries
                var fileContent = new List<List<string>>();
                var manualEntries = new List<string>();
                if (b.ID == "printIndexesBtn")
                {
                    saveIndexes_Click(new object(), new EventArgs());
                    fileContent = (List<List<string>>)ViewState["fileContent"];
                    if (fileContent == null) return;
                }
                // Or save index(es) if Manual Entries
                else if (b.ID == "saveAndPrint")
                {
                    // Check if anything was entered at all. If nothing entered, break out.
                    bool nothing = isNothingEntered();
                    if (nothing) return;

                    // Then save
                    saveIndex_Click(new object(), new EventArgs());
                    manualEntries = (List<string>)ViewState["manualEntries"];
                    if (manualEntries == null) return;
                }

                // Clear page
                formPanel.Visible = false;

                // Start writing index sheet pages
                indexPage.Visible = true;
                string indexPageString = "";
                indexPageString += "<div id = 'pageToPrint' style='margin-top:-50px;'>";

                // Write index sheet pages if File Entries
                if (b.ID == "printIndexesBtn")
                {
                    int currentCount = 0;
                    foreach (List<string> entries in fileContent)
                    {
                        currentCount++;
                        var result = writeIndexPage(entries, currentCount, fileContent.Count);
                        if (result.Item1.Contains("Error"))
                        {
                            string msg = result.Item1;
                            ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                            return;
                        }
                        else indexPageString += result.Item2;
                    }
                }
                // Or Write index sheet pages if Manual Entries
                else if (b.ID == "saveAndPrint")
                {
                    var result = writeIndexPage(manualEntries, 1, 1);
                    if (result.Item1.Contains("Error"))
                    {
                        string msg = result.Item1;
                        ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                        return;
                    }
                    else indexPageString += result.Item2;
                }

                // Close pageToPrint & wrapper div tags
                indexPageString += "</div>";

                indexPage.InnerHtml = indexPageString;

                // Finally, Print Index sheet.
                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "PrintOperation", "printing();", true);
            }
            catch (Exception ex)
            {
                Button b = (Button)sender;
                string msg = "Error 4f:  Couldn't process file. Contact system admin.";
                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);

                // Log the exception and notify system operators
                ExceptionUtility.LogException(ex);
                ExceptionUtility.NotifySystemOps(ex);
                return;
            }
        }


        // 'VIEW CONTENT' CLICKED: VIEW CVS INDEX DATA FILE.
        protected void viewContent_Click(object sender, EventArgs e)
        {   
            try 
            {
                if (viewContentBtn.Text == "Hide")
                {
                    GridView1.Visible = false;
                    viewContentBtn.Text = "View";
                }
                else
                {
                    GridView1.Visible = true;
                    viewContentBtn.Text = "Hide";
                }

                // Get file path
                var fileName = uploadHidden.Text;
                if (fileName == string.Empty)
                {
                    string msg = "Error 4g:  Could not retrieve file name. Contact system admin. ";
                    ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                    return;
                }
                string SaveLocation = Server.MapPath("App_Data") + "\\" + fileName;

                // Get column count
                int colCount = 0;
                var regexList = (List<Tuple<string, string, string>>)Session["regexList"];
                foreach (Tuple<string, string, string> controlSettings in regexList)
                {
                    if (controlSettings.Item1 != string.Empty)
                        colCount++;
                }

                //Create a DataTable.
                DataTable dt = new DataTable();
                for (int i = 0; i < colCount; i++)
                {
                    dt.Columns.Add(regexList[i].Item1, typeof(string));
                }

                //Read the contents of CSV file.
                string csvData = File.ReadAllText(SaveLocation);

                //Execute a loop over the rows.
                foreach (string row in csvData.Split('\n'))
                {
                    if (!string.IsNullOrEmpty(row))
                    {
                        dt.Rows.Add();
                        int i = 0;

                        //Execute a loop over the columns.
                        foreach (string cell in row.Split(','))
                        {
                            dt.Rows[dt.Rows.Count - 1][i] = cell;
                            i++;
                        }
                    }
                }

                //Bind the DataTable.
                GridView1.DataSource = dt;
                GridView1.DataBind();
            }
            catch(Exception ex)
            {
                string msg = "Error 33a: Issue occured while attempting to display contents. Contact system admin.";
                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                // Log the exception and notify system operators
                ExceptionUtility.LogException(ex);
                ExceptionUtility.NotifySystemOps(ex);
            }
        }


        // PREVENT LINE BREAKS
        protected void rowDataBound(object sender, GridViewRowEventArgs e)
        {
            try
            {
                // Set column borders & Prevent line breaks
                if (e.Row.RowType == DataControlRowType.Header)
                {
                    string colBorder = "border: 1px solid #717171; white-space: nowrap; font-family: Arial";
                    for (int i = 0; i < e.Row.Cells.Count; i++)
                        e.Row.Cells[i].Attributes.Add("style", colBorder);
                }

                // Set column borders & Prevent line breaks
                if (e.Row.RowType == DataControlRowType.DataRow)
                {
                    string colBorder = "border: 1px solid #cccccc; white-space: nowrap;";
                    for (int i = 0; i < e.Row.Cells.Count; i++)
                        e.Row.Cells[i].Attributes.Add("style", colBorder);
                }
            }
            catch (Exception ex)
            {
                string msg = "Error 34: Issue occured while attempting to prevent line breaks in table. Contact system admin.";
                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                // Log the exception and notify system operators
                ExceptionUtility.LogException(ex);
                ExceptionUtility.NotifySystemOps(ex);
            }
        }
        // GENERATE INDEX. HELPER
        private string generateBarcode()
        {
            try
            {
                if (!Page.IsValid) return "Error: page invalid";

                // Making the Index string
                string year = DateTime.Now.ToString("yy");
                string julianDay = DateTime.Today.DayOfYear.ToString();
                string ThreeDigitsJulian = julianDay.ToString().PadLeft(3, '0');
                string time = DateTime.Now.ToString("HHmmssfff");
                generateIndexSection.Visible = true;
                return selectJob.SelectedValue.ToUpper() + year + ThreeDigitsJulian + time;

                // Convert index to barcode
                // imgBarcode.ImageUrl = string.Format("ShowCode39Barcode.ashx?code={0}&ShowText={1}&Thickness={2}",indexString,showTextValue, 1);
            }
            catch (Exception ex)
            {
                string msg = "Error 4h: Issue occured while attempting to generate Index. Contact system admin. ";
                // Log the exception and notify system operators
                ExceptionUtility.LogException(ex);
                ExceptionUtility.NotifySystemOps(ex);
                return msg + ex.Message;
            }
        }


        // MANUAL 'SAVE INDEX' CLICKED: SAVING INDEX INTO DB.
        protected void saveIndex_Click(object sender, EventArgs e)
        {
            if (!Page.IsValid) return;
            ViewState["fileContent"] = null; // Clearing any cached file entries

            // Get operator's entries & Check regex rules
            var regexList = (List<Tuple<string, string, string>>)Session["regexList"];
            var entries = new List<string>();

            for (int i = 1; i <= 10; i++)
            {
                TextBox c = this.Master.FindControl("MainContent").FindControl("label" + i + "Box") as TextBox;
                DropDownList d = this.Master.FindControl("MainContent").FindControl("label" + i + "Dropdown") as DropDownList;
                
                if (c != null && c.Visible == true)     // If control is a textbox
                {
                    // Make sure 1st field not blank
                    if (i == 1 && c.Text == string.Empty)
                    {
                        string label = regexList[0].Item1;
                        string msg = label + " field is required.";
                        string color = "#ff3333;";
                        onScreenMsg(msg, color, "manual");
                        c.Focus();
                        return;
                    }

                    // Check regex rules
                    if (regexList[i - 1].Item2 != string.Empty)
                    {
                        string label = regexList[i - 1].Item1;
                        string pattern = @regexList[i - 1].Item2;
                        Regex r = new Regex(pattern, RegexOptions.IgnoreCase);
                        if (!r.IsMatch(c.Text))
                        {
                            string msg = label + ": " + regexList[i - 1].Item3;
                            string color = "#ff3333;";
                            onScreenMsg(msg, color, "manual");
                            c.Focus();
                            return;
                        }
                    }
                    entries.Add(c.Text);
                }
                else if (d != null && d.Visible == true)    // If control is a dropdown
                    entries.Add(d.SelectedValue);
                else
                    entries.Add(string.Empty);
            }

            // Check if anything was entered at all. If nothing entered, break out.
            bool nothing = isNothingEntered();
            if (nothing) return;

            // Get barcode
            string barcodeIndex = generateBarcode();
            if (barcodeIndex.Contains("Error"))
            {
                string msg = barcodeIndex;
                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                return;
            }

            // Temporarely disable Save Index button to prevent quick multiple saves.
            Control manualSave = Helper.GetPostBackControl(this.Page);
            if (manualSave != null && manualSave.ID == "saveIndex" && lastValuesEntered.Checked)
                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "pauseSave", "saveBtnPause();", true);

            // Save barcode and entries
            string result = saveIndexAndEntries(barcodeIndex, entries);
            if (result.Contains("Error")) return;

            // Stick barcode to the end of entries
            entries.Add(barcodeIndex);
            ViewState["manualEntries"] = entries;

            if(result.Contains("pass"))
            {
                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "fadeoutOp", "FadeOut();", true);
                ViewState["manualPrintCancelled"] = 1;
            }
        }



        // SAVE INDEX & ENTRIES. HELPER
        private string saveIndexAndEntries(string index, List<string>entries)
        {
            try
            {
                // First, get current user id via name.
                string user = HttpContext.Current.User.Identity.Name.Split('\\').Last();
                int opID = Helper.getUserId(user);
                if (opID == 0)
                {
                    string msg = "Error 05: Could not identify active user. Contact system admin.";
                    ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                    return msg;
                }

                // Then, get selected job id
                int jobID = getJobId(this.selectJob.SelectedValue);
                if (jobID == 0)
                {
                    string msg = "Error 06: Could not identify the selected job. Contact system admin.";
                    ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                    selectJob.SelectedValue = "Select";
                    return msg;
                }

                // Saving
                string barcodeIndex = index;
                using (SqlConnection con = Helper.ConnectionObj)
                {
                    using (SqlCommand cmd = con.CreateCommand())
                    {
                        cmd.CommandText = "INSERT INTO INDEX_DATA (JOB_ID, BARCODE, VALUE1, VALUE2, " +
                                          "VALUE3, VALUE4, VALUE5, VALUE6, VALUE7, VALUE8, VALUE9, VALUE10, " + 
                                          "OPERATOR_ID, CREATION_TIME, PRINTED) VALUES(@jobId, @barcodeIndex, " +
                                          "@val1, @val2, @val3, @val4, @val5, @val6, @val7, @val8, @val9, @val10, @opId, @time, @printed)";
                        cmd.Parameters.AddWithValue("@jobID", jobID);
                        cmd.Parameters.AddWithValue("@barcodeIndex", barcodeIndex);
                        for (int i = 1; i <= 10; i++)
                        {   
                            if (entries[i - 1] != string.Empty) cmd.Parameters.AddWithValue("@val" + i, entries[i - 1]);
                            else cmd.Parameters.AddWithValue("@val" + i, DBNull.Value);
                        }
                        cmd.Parameters.AddWithValue("@opId", opID);
                        cmd.Parameters.AddWithValue("@time", DateTime.Now);
                        cmd.Parameters.AddWithValue("@printed", 0);

                        con.Open();
                        if (cmd.ExecuteNonQuery() == 1)
                        {
                            Control c = Helper.GetPostBackControl(this.Page);
                            if (c != null && c.ID == "saveIndex" && lastValuesEntered.Checked)
                            {
                                string msg = "Index string saved.";
                                string color = "green;";
                                onScreenMsg(msg, color, "manual");
                                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "fadeoutOper", "FadeOut3();", true);
                            }
                            else if (c != null && c.ID == "saveIndex")
                            {
                                string msg = "Index string saved.";
                                string color = "green;";
                                onScreenMsg(msg, color, "manual");
                                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "fadeoutOp", "FadeOut();", true);
                            }

                            // Remember last values entered
                            if (!lastValuesEntered.Checked) 
                                clearFields();

                            return "pass";
                        }
                        else
                        {
                            string msg = "Error 07: Issue occured while attempting to save. Contact system admin.";
                            ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                            return msg;
                        }
                    }
                }
            }
            catch (Exception ex)
            {
                // Log the exception and notify system operators
                ExceptionUtility.LogException(ex);
                ExceptionUtility.NotifySystemOps(ex);
                if (ex.Message.Contains("Violation of UNIQUE KEY"))
                {
                    string msg = "Error 08: The Index you are trying to save already exists!";
                    ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                    return msg;
                }
                else if (ex.Message.Contains("String or binary data would be truncated"))
                {
                    string msg = "Error 8a: Entry too long! 30 characters maximum, including blank spaces.";
                    ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                    return msg;
                }
                else
                {
                    string msg = "Error 09: Issue occured while attempting to save index. Contact system admin.";
                    ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('"+ msg + "');", true);
                    return msg;
                }
            }
        }
        

        // WRITE INDEX SHEET PAGE CONTENT. HELPER
        private Tuple<string,string> writeIndexPage(List<string> indexRecord, int currentCount, int totalCount)
        {
            try
            {
                string indexPageString = "";

                //Get index string
                string indexString = indexRecord.Last();
                string jobName = indexString.Substring(0, indexString.Length - 14);
                Image imgBarcode = new Image();
                imgBarcode.ImageUrl = string.Format("ShowCode39BarCode.ashx?code={0}&ShowText=0&Height=65", indexString);

                // Write Index sheet page content if IE browser
                HttpBrowserCapabilities browser = Request.Browser;
                if (browser.Browser == "InternetExplorer" || browser.Browser == "IE")
                {
                    // Write to index page
                    indexPageString +=
                        "<div class='pageTitleIE'>" + jobName + " - Index Header" + "</div>" +
                        "<div>" +
                            "<div class='spaceIE'>" +
                                "<img class='barcode1IE' src='" + imgBarcode.ImageUrl + "'> " +
                            "</div>" +
                            "<div class='index1IE'>" + indexString + "</div>" +
                            "<div class='rotateIE'>" +
                                "<img class='barcode2IE' src='" + imgBarcode.ImageUrl + "'> " +
                                "<div class='index2IE' >" + indexString + "</div>" +
                            "</div>" +
                        "</div>";

                    // Remove extra space if it's the last page to print
                    if (totalCount == currentCount)
                    {
                        indexPageString +=
                            "<table class='dataSectionLastIE'>" +
                                "<tr>" +
                                    "<td> INDEX STRING: </td>" +
                                    "<td style='padding-left:15px;'>" + indexString.ToUpper() + "</td>" +
                                "</tr>";
                    }
                    else // Keep extra space after page if not last page
                    {
                        indexPageString +=
                           "<table class='dataSectionIE'>" +
                                "<tr>" +
                                    "<td> INDEX STRING: </td>" +
                                    "<td style='padding-left:15px;'>" + indexString.ToUpper() + "</td>" +
                                "</tr>";
                    }
                    var regexList = (List<Tuple<string, string, string>>)Session["regexList"];

                    for (int i = 0; i < regexList.Count; i++)
                    {
                        if (indexRecord[i] != string.Empty)
                        {
                            string label = regexList[i].Item1;
                            indexPageString += 
                                "<tr>" +
                                    "<td>" + label.ToUpper() + ":" + "</td>" +
                                    "<td style='padding-left:15px;;'>" + indexRecord[i].ToUpper() + "</td>" +
                                "</tr>";
                        }
                    }
                    indexPageString += 
                                "<tr>" +
                                    "<td>DATE PRINTED: </td>" +
                                    "<td style='padding-left:15px;'>" + DateTime.Now + "</td>" +
                                "</tr>" +
                            "</table>";
                    return Tuple.Create("pass",indexPageString);
                }
                else // Chrome browser
                {
                    // Write Index sheet page content
                    indexPageString +=
                        "<div class='pageTitle'>" + jobName + " - Index Header" + "</div>" +
                        "<div>" +
                            "<div class='space'>" +
                                "<img class='barcode1' src='" + imgBarcode.ImageUrl + "'> " +
                            "</div>" +
                            "<div class='index1'>" + indexString + "</div>" +
                            "<div class='rotate'>" +
                                "<img class='barcode2' src='" + imgBarcode.ImageUrl + "'> " +
                                "<div class='index2' >" + indexString + "</div>" +
                            "</div>" +
                        "</div>";

                    // Remove extra space if it's the last page to print
                    if (currentCount == totalCount)
                    {
                        indexPageString +=
                            "<table class='dataSectionLast'>" +
                                "<tr style='padding:0px 0px 0px 0px;'>" +
                                    "<td> INDEX STRING: </td>" +
                                    "<td style='padding-left:15px;'>" + indexString.ToUpper() + "</td>" +
                                "</tr>";
                    }
                    else // Keep extra space after page if not last page
                    {
                        indexPageString +=
                           "<table class='dataSection'>" +
                                "<tr>" +
                                    "<td> INDEX STRING: </td>" +
                                    "<td style='padding-left:15px;'>" + indexString.ToUpper() + "</td>" +
                                "</tr>";
                    }

                    var regexList = (List<Tuple<string, string, string>>)Session["regexList"];

                    for (int i = 0; i < regexList.Count; i++)
                    {
                        if (indexRecord[i] != string.Empty)
                        {
                            string label = regexList[i].Item1;
                            indexPageString += 
                                "<tr>" +
                                    "<td>" + label.ToUpper() + ":" + "</td>" +
                                    "<td style='padding-left:15px;'>" + indexRecord[i].ToUpper() + "</td>" +
                                "</tr>";
                        }
                    }
                    indexPageString += 
                                "<tr>" +
                                    "<td>DATE PRINTED: </td>" +
                                    "<td style='padding-left:15px;'>" + DateTime.Now + "</td>" +
                                "</tr>" +
                            "</table>";
                    return Tuple.Create("pass", indexPageString);
                }
            }
            catch(Exception ex)
            {
                // Log the exception and notify system operators
                ExceptionUtility.LogException(ex);
                ExceptionUtility.NotifySystemOps(ex);
                string msg = "Error: " + ex.Message;
                return Tuple.Create(msg, "");
            }
        }



        // SET INDEX AS PRINTED IN DB. 
        protected string setIndexAsPrinted(string index)
        {
            try
            {
                if (!Page.IsValid) return "Error: Page invalid";
                var counter = 0;
                string indexString = index;
                using (SqlConnection con = Helper.ConnectionObj)
                {
                    using (SqlCommand cmd = con.CreateCommand())
                    {
                        cmd.CommandText = "UPDATE INDEX_DATA SET PRINTED=@printed WHERE BARCODE=@barcodeIndex";
                        cmd.Parameters.AddWithValue("@printed", 1);
                        cmd.Parameters.AddWithValue("@barcodeIndex", indexString);
                        con.Open();
                        int result = cmd.ExecuteNonQuery();
                        if (result == 1)
                        {
                            counter++;
                        }
                        else
                        {
                            string msg = "Error 11: Index saved, but issue occured while attempting to set it to PRINTED. Contact system admin.";
                            ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                            clearFields();
                            return msg;
                        }

                        // Confirmation msg & back to unprinted indexes gridview
                        if (counter == 1)
                        {
                            return "pass";
                        }
                        else
                        {
                            string msg = "Error 12: Index saved, but issue occured while attempting to set it to PRINTED. Contact system admin.";
                            ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                            clearFields();
                            return msg;
                        }
                    }
                }
            }
            catch (Exception ex)
            {
                string msg = "Error 13: Index saved, but issue occured while attempting to set it to PRINTED. Contact system admin.";
                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                // Log the exception and notify system operators
                ExceptionUtility.LogException(ex);
                ExceptionUtility.NotifySystemOps(ex);
                return msg;
            }
        }



        // SET INDEX AS PRINTED AFTER INDEX SHEET PRINTOUT 
        protected void setAsPrinted()
        {
            try
            {
                formPanel.Visible = true;
                satisfied.Visible = false;
                linkToUnprinted.Visible = true;
                formPanelJobSelection.Visible = true;
                indexCreationSection.Visible = true;

                // For manual entries
                if (ViewState["manualEntries"] != null)
                {
                    var manualEntries = (List<string>)ViewState["manualEntries"];
                    string result = setIndexAsPrinted(manualEntries.Last());
                    if (result.Contains("Error"))
                    {
                        string msg1 = result;
                        ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg1 + "');", true);
                        return;
                    }
                    string msg = "Index string saved and set as PRINTED.";
                    string color = "green;";
                    onScreenMsg(msg, color, "manual");
                    clearFields();
                    ScriptManager.RegisterStartupScript(Page, Page.GetType(), "fadeoutOp", "FadeOut();", true);
                }

                // For uploaded file
                else if (ViewState["fileContent"] != null)
                {
                    var fileContent = (List<List<string>>)ViewState["fileContent"];
                    int countPass = 0;
                    foreach (List<string> entries in fileContent)
                    {
                        string result = setIndexAsPrinted(entries.Last());
                        if (result.Contains("Error"))
                        {
                            string msg1 = result;
                            ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg1 + "');", true);
                            return;
                        }
                        else if (result == "pass") countPass++;
                    }
                    string msg = countPass + " index string(s) saved and set as PRINTED.";
                    string color = "green;";
                    onScreenMsg(msg, color, "file");
                    ScriptManager.RegisterStartupScript(Page, Page.GetType(), "fadeoutOp", "FadeOut2();", true);
                }

                Panel p = Master.FindControl("footerSection") as Panel;
                p.Visible = true;
            }
            catch (Exception ex)
            {
                string msg = "Error 14: Issue occured while attempting to set job as PRINTED. Contact system admin." ;
                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                // Log the exception and notify system operators
                ExceptionUtility.LogException(ex);
                ExceptionUtility.NotifySystemOps(ex);
            }
        }


        // GO TO QUESTION
        protected void goToQuestion_Click(object sender, EventArgs e)
        {
            indexPage.Visible = false;
            formPanel.Visible = true;
            satisfied.Visible = true;
            linkToUnprinted.Visible = false;
            formPanelJobSelection.Visible = false;
            indexCreationSection.Visible = false;
            Panel p = Master.FindControl("footerSection") as Panel;
            p.Visible = true;
        }



        // BACK TO BLANK FORM
        protected void backToForm()
        {
            formPanel.Visible = true;
            satisfied.Visible = false;
            linkToUnprinted.Visible = true;
            formPanelJobSelection.Visible = true;
            indexCreationSection.Visible = true;
            
            ViewState["manualEntries"] = null;
            ViewState["fileContent"] = null;
            TextBox t = this.Master.FindControl("MainContent").FindControl("label1Box") as TextBox;
            if (t.Visible) t.Focus();

            if(ViewState["filePrintCancelled"] != null)
            {
                int countPass = (int)ViewState["filePrintCancelled"];

                string msg = countPass + " index string(s) saved but not set as PRINTED";
                string color = "green;";
                onScreenMsg(msg, color, "file");
                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "fadeoutOp", "FadeOut2();", true);
            }

            if (ViewState["manualPrintCancelled"] != null)
            {
                string msg = "Index string saved but not set as PRINTED";
                string color = "green;";
                onScreenMsg(msg, color, "manual");
                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "fadeoutOp", "FadeOut();", true);
            }

            Panel p = Master.FindControl("footerSection") as Panel;
            p.Visible = true;
        }



        // (HIDDEN) 'GENERATE JOBS' CLICKED: GET YOUR ACCESSIBLE JOBS.
        protected void selectJob_Click(Object sender, EventArgs e)
        {
            try
            {   
                // First, get current user id via name.
                string user = HttpContext.Current.User.Identity.Name.Split('\\').Last();
                List<int> jobIdList = new List<int>();
                noJobsFound.Visible = false;
                int opID = Helper.getUserId(user);
                if (opID == 0)
                {
                    noJobsFound.Visible = true;
                    return;
                }

                // Check if current operator is Admin &
                // Then, get all appropriate jobs for current operator from OPERATOR_ACCESS.
                using (SqlConnection con = Helper.ConnectionObj)
                {
                    using (SqlCommand cmd = con.CreateCommand())
                    {
                        // Get operator's Admin status
                        cmd.CommandText = "SELECT ADMIN FROM OPERATOR WHERE ID = @userId";
                        cmd.Parameters.AddWithValue("@userId", opID);
                        con.Open();
                        object result = cmd.ExecuteScalar();
                        bool isAdmin =false;
                        if (result != null)
                            isAdmin = (bool)cmd.ExecuteScalar();
                        cmd.Parameters.Clear(); 

                        // If Admin, get all jobs    
                        if (isAdmin ==  true)
                        {
                            cmd.CommandText =   "SELECT ABBREVIATION " +
                                                "FROM JOB " +
                                                "WHERE ACTIVE=1 " +
                                                "ORDER BY ABBREVIATION ASC";
                        }
                        else // Else get assigned jobs only.
                        {
                            cmd.CommandText =   "SELECT ABBREVIATION " +
                                                "FROM JOB " +
                                                "JOIN OPERATOR_ACCESS ON JOB.ID=OPERATOR_ACCESS.JOB_ID " +
                                                "WHERE ACTIVE=1 AND OPERATOR_ID=@userId " +
                                                "ORDER BY ABBREVIATION ASC";
                            cmd.Parameters.AddWithValue("@userId", opID);
                        }

                        using (SqlDataReader reader = cmd.ExecuteReader())
                        {
                            if (reader.HasRows)
                            {
                                while (reader.Read())
                                {
                                    string jobAbb = (string)reader.GetValue(0);
                                    selectJob.Items.Add(jobAbb);
                                }
                            }
                            else
                            {   
                                // If no accessible jobs found, display message.
                                noJobsFound.Visible = true;
                                return;
                            }
                        }
                    }
                }
            }
            catch (Exception ex)
            {
                string msg = "Error 17: Issue occured while attempting to retrieve jobs accessible to you. Contact system admin.";
                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                // Log the exception and notify system operators
                ExceptionUtility.LogException(ex);
                ExceptionUtility.NotifySystemOps(ex);
            }
        }



        // SET COLOR FOR DROPDOWN CONFIGURED JOB ITEMS.
        private void setDropdownColor()
        {
            try
            {
                using (SqlConnection con = Helper.ConnectionObj)
                {
                    using (SqlCommand cmd = con.CreateCommand())
                    {
                        cmd.CommandText = "SELECT ABBREVIATION " +
                                          "FROM JOB " +
                                          "INNER JOIN JOB_CONFIG_INDEX ON JOB.ID = JOB_CONFIG_INDEX.JOB_ID";
                        con.Open();
                        using (SqlDataReader reader = cmd.ExecuteReader())
                        {
                            if (reader.HasRows)
                            {
                                while (reader.Read())
                                {
                                    foreach (ListItem item in selectJob.Items)
                                    {
                                        if (item.Value == (string)reader.GetValue(0))
                                            item.Attributes.Add("style", "color:#009900;font-weight:bold;");
                                    }
                                }
                            }
                        }
                    }
                }
            }
            catch (Exception ex)
            {
                string msg = "Error 18: Issue occured while attempting to color configured jobs in dropdown. Contact system admin.";
                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                // Log the exception and notify system operators
                ExceptionUtility.LogException(ex);
                ExceptionUtility.NotifySystemOps(ex);
            }
        }


        // GET JOB ID VIA SELECTED JOB ABBREV.
        private int getJobId(string jobAbb)
        {
            try 
            {
                int jobID = 0;
                using(SqlConnection con = Helper.ConnectionObj) 
                {
                    using (SqlCommand cmd = con.CreateCommand()) 
                    {
                        cmd.CommandText = "SELECT ID FROM JOB WHERE ABBREVIATION = @abb";
                        cmd.Parameters.AddWithValue("@abb", jobAbb);
                        con.Open();
                        var result = cmd.ExecuteScalar();
                        if (result != null)
                        {
                            jobID = (int) result; 
                            return jobID;
                        }
                        else return jobID;
                    }
                }  
            } 
            catch(Exception ex) 
            {
                string msg = "Error 01: Issue occured while attempting to identify the selected Job. Contact system admin.";
                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                // Log the exception and notify system operators
                ExceptionUtility.LogException(ex);
                ExceptionUtility.NotifySystemOps(ex);
                return 0;
            }
        }



        // CLEAR TEXT FIELDS.
        private void clearFields()
        {
            try
            {
                List<TextBox> textBoxList = new List<TextBox>();
                textBoxList.AddRange(new TextBox[] { label1Box, label2Box, label3Box, label4Box, label5Box, label6Box, label7Box, label8Box, label9Box, label10Box, });
                List<DropDownList> dropdownsList = new List<DropDownList>();
                dropdownsList.AddRange(new DropDownList[] { label1Dropdown, label2Dropdown, label3Dropdown, label4Dropdown, label5Dropdown, 
                                                            label6Dropdown, label7Dropdown, label8Dropdown, label9Dropdown, label10Dropdown, });

                foreach (var textBox in textBoxList)
                {
                    if (textBox.Visible == true) textBox.Text = string.Empty;
                }
                foreach (var dropdown in dropdownsList)
                {
                    if (dropdown.Visible == true) dropdown.SelectedValue = "";
                }
            }
            catch (Exception ex)
            {
                string msg = "Error 19: Issue occured while attempting to clear text fields. Contact system admin.";
                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                // Log the exception and notify system operators
                ExceptionUtility.LogException(ex);
                ExceptionUtility.NotifySystemOps(ex);
            }
        }


        // PRINT VARIOUS MSG ON SCREEN INSTEAD OF A POPUP
        private void onScreenMsg(string msg, string color, string from)
        {
            var screenMsg = new TableCell();
            var screenMsgRow = new TableRow();
            screenMsg.Text = msg;
            screenMsg.Attributes["style"] = "font-size:14px; color:" + color;
            screenMsgRow.Cells.Add(screenMsg);
            if (from == "file")
            { 
                fileEntryMsg.Rows.Add(screenMsgRow);
                if (fileEntryMsg.Rows.Count == 2) fileMsgDiv.Attributes["style"] = "margin-top:5px; height:40px;";
            }
            else if (from == "manual") manualEntryMsg.Rows.Add(screenMsgRow);
        }

        // 'YES' OR 'NO' CLICKED.
        protected void satisfied_Click(object sender, EventArgs e)
        {
            try
            {
                uploadedFileMenu.Visible = false;
                Control c = sender as LinkButton;
                if (c != null)
                {
                    if (c.ID == "yesBtn")
                        setAsPrinted();
                    else 
                        backToForm();
                    satisfied.Visible = false;
                }
            }
            catch (Exception ex)
            {
                Response.Redirect("~/ErrorPage.aspx",false);

                // Log the exception and notify system operators
                ExceptionUtility.LogException(ex);
                ExceptionUtility.NotifySystemOps(ex);
            }
        }


        // CHECK IF NOTHING WAS ENTERED IN MANUAL ENTRIES.
        private bool isNothingEntered()
        {
            try
            {
                bool noEntry = true;
                for (int i = 1; i <= 10; i++)
                {
                    TextBox c = this.Master.FindControl("MainContent").FindControl("label" + i + "Box") as TextBox;
                    DropDownList d = this.Master.FindControl("MainContent").FindControl("label" + i + "Dropdown") as DropDownList;

                    if ((c != null && c.Visible == true && c.Text != string.Empty) || (d != null && d.Visible == true && d.SelectedValue != string.Empty))
                        noEntry = false;
                }
                if (noEntry == true)
                {
                    string msg = "All entries can not be blank. At least one index data must be entered.";
                    string color = "#ff3333;";
                    onScreenMsg(msg, color, "manual");
                    return true;
                }
                else return false;
            }
            catch(Exception ex)
            {
                // Log the exception and notify system operators
                ExceptionUtility.LogException(ex);
                ExceptionUtility.NotifySystemOps(ex);
                return true;
            }
        }
    }
}

------------Settings.aspx
<%@ Page Title="Settings" Language="C#" MasterPageFile="~/Site.Master" AutoEventWireup="true" CodeBehind="Settings.aspx.cs" Inherits="BarcodeConversion.Contact" %>

<asp:Content ID="BodyContent" ContentPlaceHolderID="MainContent" runat="server">
    <script>
        function FadeOut() {
            $("table[id$='jobAccessMsg2']").delay(2000).fadeOut(1000);    
        }
        function FadeOut2() {
            $("table[id$='UserSectionMsg']").delay(2000).fadeOut(1000);
        }
        function FadeOut4() {
            $("table[id$='jobSetupMsg']").delay(2000).fadeOut(1000);
        }

        function scrollToBottom() {
            $({ myScrollTop: window.pageYOffset }).animate({ myScrollTop: 600 }, {
                duration: 600,
                easing: 'swing',
                step: function (val) {
                    window.scrollTo(0, val);
                }
            });
        }
    </script>

     <asp:UpdatePanel ID="SettingsPanel" Visible="false" runat="server" UpdateMode="Conditional">
        <ContentTemplate>
        <div id="scroll">
        <div style="margin-top:45px; margin-bottom:30px; height:50px; border-bottom:solid 1px green;width:860px;">
            <table style="width:860px;">
                <tr>
                    <td><h2 style="display:inline;padding-top:25px; color:#595959;">Settings</h2></td>
                    <td style="text-align:right;"> 
                        <%-- COLLAPSE ALL--%>
                        <div style="display:block;margin-left:710px;padding-top:15px;">
                            <asp:linkButton ID="upArrow" Visible="false" runat="server" ForeColor="#737373" OnClick="collapseAll_Click" >
                                <i class="fa fa-angle-double-up" style="font-size:30px;" BackColor="#e6f3ff" runat="server" ></i>
                            </asp:linkButton>
                            <asp:linkButton ID="downArrow" Visible="true" runat="server" ForeColor="#737373" OnClick="collapseAll_Click" >
                                <i class=" fa fa-angle-double-down" style="font-size:30px;" BackColor="#e6f3ff" runat="server" ></i>
                            </asp:linkButton>
                        </div>
                    </td>
                </tr>
            </table>
        </div>

        <table>
            <tr>
                <td style="width:511px;">
                     <%-- JOB SETUP SECTION --%>
                    <div style="width:284px;border:solid 3px #808080;border-radius:3px;text-align:center;">
                        <asp:linkButton ID="newJobBtn" style="background-color:#e6e6e6;color:#666666;padding-top:2px;" Font-Underline="false" Visible="true" Width="278px" Height="29" runat="server" OnClick="newJobShow_Click">
                        <i class="fa fa-folder-open" aria-hidden="true" style="margin-right:9px;"></i> Job Setup Section&nbsp;</asp:linkButton>
                    </div>
                </td>
                <td style="width:324px;">
                     <%--USER & PERMISSION SECTION --%>
                    <div style="width:284px;border:solid 3px #808080;border-radius:3px;text-align:center;">
                        <asp:linkButton ID="newUserBtn" style="background-color:#e6e6e6;color:#666666;padding-top:2px;" Font-Underline="false" Visible="true" Width="278px" Height="29" runat="server" OnClick="permissionsShow_Click">
                        <i class="fa fa-user" aria-hidden="true" style="margin-right:10px;"></i> User & Permission Section</asp:linkButton>
                    </div>
                </td>
            </tr>   
            <tr>
                <td style="vertical-align:top;">
                    <%-- JOB SETUP SECTION BODY --%>
                    <div style="display:block;" class="auto-style5">
                        <asp:Panel ID="jobSection" Visible="false" runat="server" Width="408px" >
                            <asp:Panel CssClass="card" style="margin-top:25px;background-color:aliceblue;width:76%;padding: 0px 10px 0px 10px;" runat="server">
                                <table style="width:100%;" >
                                    <tr style="height:35px;">
                                        <th style="color:#737373;font-family:Arial;font-size:15px">&nbsp;Create / Edit Jobs</th>
                                        <td style="text-align:right;padding-right:5px;">
                                            <asp:linkButton ID="LinkButton1" runat="server" ForeColor="#737373" 
                                                OnClientClick="return alert('Notes:\n*   You can make a new job accessible to an operator right away.\n*    Jobs made accessible in this section will be visible to the specified operator but can not be processed until configured in the Index Config section below.\n*   When selecting a job to edit, Active jobs are in green.')">
                                                <i class="fa fa-info-circle" style="font-size:20px;" BackColor="#e6f3ff" runat="server" ></i>
                                            </asp:linkButton></td>
                                    </tr>
                                </table>
                            </asp:Panel> 
                            
                            <table  style="margin-top:25px; width: 76%; margin-right: 36px; height: 149px;"  class=auto-style3 > 
                                <tr>
                                    <td style="padding-bottom:15px;"><asp:Label Text="Choose Action: " runat="server"></asp:Label></td>
                                    <td style="padding-bottom:15px;">
                                        <asp:DropDownList ID="selectAction" AutoPostBack="True" onmousedown="this.focus()" runat="server" OnSelectedIndexChanged="actionChange">
                                            <asp:ListItem Selected="true" Value="create">Create New Job</asp:ListItem>
                                            <asp:ListItem Value="edit">Edit Existing Job</asp:ListItem>
                                            <%--<asp:ListItem Value="delete">Delete Existing Job</asp:ListItem>--%>
                                        </asp:DropDownList>
                                    </td>
                                </tr>
                                <tr>
                                    <td class="auto-style2" style="height: 35px; width: 290px;"><asp:Label runat="server">Job Abbreviation: </asp:Label></td>
                                    <td style="height: 35px">
                                        <asp:TextBox ID="jobAbb" placeholder=" Required" runat="server"></asp:TextBox>
                                        <asp:DropDownList ID="selectJobList" onmousedown="this.focus()" runat="server">
                                            <asp:ListItem Value="Select">Select</asp:ListItem>
                                        </asp:DropDownList>
                                    </td>
                                </tr> 
                                <tr>
                                    <td class="auto-style2" style="width: 286px"><asp:Label ID="jobNameLabel" runat="server">Job Name: </asp:Label></td>
                                    <td><asp:TextBox ID="jobName" placeholder=" Required" runat="server"></asp:TextBox></td>
                                </tr>
                                <tr>
                                    <td class="auto-style2" style="padding-top:5px; width: 286px;"><asp:Label ID="jobActiveLabel" runat="server">Active: </asp:Label></td>
                                    <td>
                                        <asp:DropDownList ID="jobActiveBtn" style="margin-top:5px;" AutoPostBack="True" onmousedown="this.focus()" OnSelectedIndexChanged="onActiveSelect" runat="server">
                                            <asp:ListItem Selected="True" Value="1">True</asp:ListItem>
                                            <asp:ListItem Value="0">False</asp:ListItem>
                                        </asp:DropDownList>
                                    </td>
                                </tr>
                                 <tr>
                                    <td class="auto-style2" style="padding-top:22px; width: 286px;"><asp:Label ID="jobAssignedToLabel" runat="server">Grant Access To: </asp:Label></td>
                                    <td>
                                        <asp:DropDownList ID="jobAssignedTo" style="margin-top:22px;" onmousedown="this.focus()" runat="server">
                                            <asp:ListItem Selected="true" Value="Select">Select</asp:ListItem>
                                        </asp:DropDownList>
                                    </td>
                                </tr>
                
                            </table>
                            <table class="auto-style4">
                                <tr>
                                   <td style="text-align:right;">
                                        <asp:Button ID="deleteJobBtn" CssClass="btn btn-primary" Visible="false" runat="server" Text="Delete " 
                                            OnClientClick="return confirm('ATTENTION!\n\nDeleting this job will also delete its configuration, all indexes associated to it, and any other related records in other entities. Unless it is a must, we advise to Deactivate job instead.\n\nDo you still want to procede with Delete?');" OnClick="deleteJob_Click"/> </td>
                                </tr>
                                <tr>
                                    <td><asp:Table id="jobSetupMsg" style="float:left;" runat="server"></asp:Table> </td>
                                    <td style="height: 15px; text-align:right;">
                                        <asp:Button ID="editJobBtn" CssClass="btn btn-primary" style="padding:1px 10px 0px 10px;" Visible="false" Font-Size="10" runat="server" Text="Edit " OnClick="editJob_Click" />
                                        <asp:Button ID="createJobBtn" CssClass="btn btn-primary" style="padding:1px 10px 0px 10px;" Visible="true" Font-Size="10" runat="server" Text="Create" OnClick="createJob_Click"/>
                                    </td>
                                </tr>
                            </table>
           
                        </asp:Panel>
                    </div>
                </td>
                <td style="vertical-align:top;">
                    <%--USER & PERMISSION SECTION BODY--%>
                    <div style="display:inline-block; width: 26%;" class="auto-style5">
                        <asp:Panel ID="newUserSection" Visible="false" runat="server" Width="322px" Height="250px" style="margin-top: 0px" >
                            <asp:Panel CssClass="card" style="margin-top:25px;background-color:aliceblue;width:97%;padding: 0px 10px 0px 10px;" runat="server">
                                <table style="width:100%;">
                                    <tr style="height:35px;">
                                        <th style="color:#737373;font-family:Arial;font-size:15px">&nbsp;Add Operators & Admins</th>
                                        <td style="text-align:right;padding-right:5px;">
                                            <asp:linkButton ID="LinkButton5" runat="server" ForeColor="#737373" 
                                                OnClientClick="return alert('Notes:\n*  Anyone accessing the site for the 1st time is automatically added as an operator.\n*  An operator can also be added prior accessing the site.\nTo add, just type in the operator\'s username, set Permissions & submit.\n*    You can also change existing operator\'s permissions.')">
                                                <i class="fa fa-info-circle" style="font-size:20px;" BackColor="#e6f3ff" runat="server" ></i>
                                            </asp:linkButton></td>
                                    </tr>
                                </table>
                            </asp:Panel>
                            
                            <table  style="margin-top:25px; height:72px;"  class=auto-style3 >
                                <tr>
                                    <td class="auto-style2" style="height: 31px; margin-left: 200px;"><asp:Label runat="server">Operator: </asp:Label></td>
                                    <td style="height: 31px"><asp:TextBox ID="user" placeholder=" Required" runat="server"></asp:TextBox></td>
                                </tr> 
                                <tr>
                                    <td class="auto-style2"><asp:Label runat="server">Permissions: </asp:Label></td>
                                    <td>
                                        <asp:DropDownList ID="permissions" onmousedown="this.focus()" runat="server">
                                            <asp:ListItem Selected="true" Value="0">Operator</asp:ListItem>
                                            <asp:ListItem Value="1">Admin</asp:ListItem>
                                        </asp:DropDownList>
                                    </td>
                                </tr>
                            </table>
                            <table style="width:315px;">
                                <tr>
                                    <td><asp:Table id="UserSectionMsg" style="float:left;" runat="server"></asp:Table></td>
                                    <td><asp:Button ID="submitOp" CssClass="btn btn-primary" style="padding:1px 10px 0px 10px;float:right;" Visible="true" Font-Size="10" runat="server" Text="Submit" OnClick="setPermissions_Click"/></td>
                                </tr>
                            </table>
                        </asp:Panel>
                    </div>
                </td>
            </tr>
            <tr>
                <td colspan="2">
                     <div id="line" visible="false" style=" height:30px; border-bottom:solid 1px green;width:860px;" runat="server"></div>
                </td>
            </tr>
            <tr>
                <td>
                    <%--JOB ACCESS SECTION --%>
                    <div style="width:284px;border:solid 3px #808080;border-radius:3px;text-align:center;margin-top:30px;">
                        <asp:linkButton ID="assignBtn" style="background-color:#e6e6e6;color:#666666;padding-top:2px;" Font-Underline="false" Visible="true" Width="278px" Height="29" runat="server" OnClick="assignShow_Click">
                        <i class="fa fa-unlock-alt" aria-hidden="true" style="margin-right:10px;"></i> Job Access Section</asp:linkButton>
                    </div>
                </td>
                <td>
                    <%--JOB INDEX CONFIG SECTION --%>
                    <div style="width:284px;border:solid 3px #808080;border-radius:3px;text-align:center;margin-top:30px;">
                        <asp:linkButton ID="jobIndexEditingBtn" style="background-color:#e6e6e6;color:#666666;padding-top:2px;" Font-Underline="false" Visible="true" Width="278px" Height="29" runat="server" OnClick="jobIndexEditingShow_Click">
                        <i class="fa fa-cogs" aria-hidden="true" style="margin-right:10px;"></i> Index Configuration Section</asp:linkButton>
                    </div>
                </td>
            </tr>
            <tr>
                <td style="vertical-align:top;">
                    <%--JOB ACCESS SECTION BODY --%>
                    <asp:Panel ID="assignPanel" Visible="false" runat="server">
                        <asp:Panel CssClass="card" style="margin-top:25px;background-color:aliceblue;width:315px;padding: 0px 10px 0px 10px;" runat="server">
                            <table style="width:100%;">
                                <tr style="height: 35px;">
                                    <th style="color:#737373;font-family:Arial;font-size:15px">&nbsp;Assign Jobs to Operators</th>
                                    <td style="text-align:right;padding-right:5px;">
                                        <asp:linkButton ID="LinkButton3" runat="server" ForeColor="#737373" 
                                            OnClientClick="return alert('Notes:\n*   Jobs made accessible in this section will be visible to the specified operator but can not be processed until configured in the Index Config section.')">
                                            <i class="fa fa-info-circle" style="font-size:20px;" BackColor="#e6f3ff" runat="server" ></i>
                                        </asp:linkButton></td>
                                </tr>
                             </table>
                        </asp:Panel>
                        
                        <table  style="height: 42px; margin-bottom:10px; margin-top:10px; width: 315px;"  class=auto-style3 >
                            <tr>
                                <td class="auto-style2" style="height:31px;width:125px;"><asp:Label runat="server">Operator: </asp:Label></td>
                                <td style="height:25px;text-align:left;">
                                    <asp:DropDownList ID="assignee" onmousedown="this.focus()" AutoPostBack="true" OnSelectedIndexChanged="getInaccessibleJobs" runat="server">
                                        <asp:ListItem Selected="true" Value="Select">Select</asp:ListItem>
                                    </asp:DropDownList>
                                </td>
                                <td><asp:Table id="jobAccessMsg1" style="float:left;" runat="server"></asp:Table></td>
                            </tr> 
                        </table>
                        <asp:Panel CssClass="card" style="margin-bottom:10px;background-color:aliceblue;width:315px;padding: 0 5px 0px 12px;" runat="server">
                            <table >
                                <tr style="height:40px;">
                                    <td style="height:10px; text-align:left;padding-right:10px;">
                                        <asp:Button ID="assignedBtn" CssClass="btn btn-primary" style="padding:2px 10px 1px 10px;"  Visible="true" Font-Size="9" runat="server" Text="Accessible" OnClick="assignedJob_Click" /></td>
                                    <td style="height:10px; text-align:center;padding-right:10px;">
                                        <asp:Button ID="inaccessibleBtn" CssClass="btn btn-primary" style="padding:2px 10px 1px 10px;" Font-Size="9" Visible="true" runat="server" Text="Inaccessible" OnClick="unassignedJob_Click" /></td>
                                    <td style="height:10px; text-align:right;">
                                        <asp:Button ID="unassignedBtn" CssClass="btn btn-primary" style="padding:2px 10px 1px 10px;" Visible="true" runat="server" Font-Size="9" Text="Active " OnClick="unassignedJob_Click"/></td>
                                </tr> 
                            </table>
                        </asp:Panel>
                        
                        <table style="width:315px;" runat="server">
                            <tr>
                                <td><asp:Label ID="jobsLabel" Font-Size="14" Text="Active Jobs" runat="server"></asp:Label></td>
                                <td><asp:Table id="jobAccessMsg2" style="float:right;" runat="server"></asp:Table></td>
                            </tr>
                        </table>
                        
                        <asp:GridView ID="jobAccessGridView" runat="server" style="margin-top:4px;" CssClass="settingsGridview" PagerStyle-CssClass="pager"
                                    PageSize="10" HeaderStyle-CssClass="header" RowStyle-CssClass="rows" AllowPaging="true" OnPageIndexChanging="pageChange_Click"
                                    OnRowDataBound="rowDataBound"> 
                            <columns>             
                                <asp:templatefield HeaderText="Select">
                                    <HeaderTemplate>
                                        &nbsp;&nbsp;<asp:checkbox ID="selectAll" AutoPostBack="true" OnCheckedChanged="selectAll_changed" runat="server"></asp:checkbox>
                                    </HeaderTemplate>
                                    <ItemTemplate>
                                        &nbsp;<asp:checkbox ID="cbSelect"  runat="server"></asp:checkbox>
                                    </ItemTemplate>
                                </asp:templatefield>

                                <asp:templatefield HeaderText ="&nbsp;N&#176;" ShowHeader="true">
                                    <ItemTemplate >
                                        <%# Container.DataItemIndex + 1 %>
                                    </ItemTemplate>
                                </asp:templatefield>
                            </columns>       
                        </asp:GridView>   
                        
                        <div style="display:block; width:315px;" >
                            <table style="margin-top:11px; width:315px;">
                                <tr>
                                    <td style="text-align:right;width: 315px;">
                                        <asp:Button ID="deleteAssignedBtn" CssClass="btn btn-danger" style="padding:1px 10px 0px 10px;" Visible="false" runat="server" Font-Size="10" Text="Deny" OnClick="deleteAssigned_Click"/>
                                        <asp:Button ID="jobAccessBtn" CssClass="btn btn-primary" style="padding:1px 10px 0px 10px;" Visible="false" runat="server" Font-Size="10" Text="Grant" OnClick="jobAccess_Click" />
                                    </td>                 
                                </tr>                  
                            </table>
                        </div>   
                    </asp:Panel>
                </td>
                <td style="vertical-align:top;">
                    <%--JOB INDEX CONFIG BODY --%>
                    <asp:Panel ID="jobIndexEditingPanel" Visible="false" runat="server">
                        <asp:Panel CssClass="card" style="margin-top:25px;background-color:aliceblue;width:315px;padding: 0px 5px 0px 10px;" runat="server">
                            <table style="width:100%;">
                                <tr style="height:35px;">
                                    <th style="color:#737373;font-family:Arial;font-size:15px">&nbsp;Create Index Data Controls for Jobs</th>
                                    <td style="text-align:right;padding-right:5px;">
                                        <asp:linkButton ID="LinkButton4" runat="server" ForeColor="#737373" 
                                            OnClientClick="return alert('Notes:\n*  Only jobs configured in this section can be processed by operators.\n*  Already configured jobs are in green.\n*  Regex is optional. But if specified, an alert message must also be specified to let operator know what a valid entry should be.\n*  Example: Regex:  .*\\S.*    Alert: Field can not be empty!\n*  Unspecified regex means textbox can be left blank when creating index.\n*  One can make use of \'regexr.com\' to test regular expressions.')">
                                            <i class="fa fa-info-circle" style="font-size:20px;" BackColor="#e6f3ff" runat="server" ></i>
                                        </asp:linkButton></td>
                                </tr>
                            </table>
                        </asp:Panel>
                        
                        <table class=table style="width:297px;margin-top:15px;">
                            <tr>
                                <td style="width:150px;white-space:nowrap;"><asp:Label ID="selectJobLabel" runat="server">Job Abbreviation:</asp:Label></td>
                                <td style="text-align:left;padding-left:20px;"> 
                                    <asp:DropDownList ID="selectJob" runat="server" AutoPostBack="true" onmousedown="this.focus()" OnSelectedIndexChanged="JobAbbSelect">
                                        <asp:ListItem Value="Select">Select</asp:ListItem>
                                    </asp:DropDownList>
                                </td>
                            </tr>
                        </table>

                        <%--<table>
                          <% for (int i = 0; i < 10; i++){ %>
                            <tr><td><%= i %></td></tr>
                          <% } %>
                        </table>--%>

                        <table id="labelsTable" visible="false" style="margin-top:15px;margin-bottom:5px;width:315px;" class=auto-style3 runat="server">
                             
                            <tr style="height:35px;">
                                <td style="width: 80px;"><asp:Label ID="lab1" Visible="true" Height="30" Text="LABEL1:" runat="server"></asp:Label></td>
                                <td style="text-align:right;"><asp:TextBox ID="label1" Visible="true" ReadOnly="true" placeholder="  Required only for Set" onfocus="this.select()" runat="server" Width="200px"></asp:TextBox></td>
                                <td style="padding-left:5px;padding-top:4px;">
                                    <asp:linkButton ID="edit1" Visible="true" runat="server" ForeColor="#737373" OnClick="processRequest" >
                                        <i class="fa fa-pencil-square-o" style="font-size:22px;" BackColor="#e6f3ff" runat="server" ></i>
                                    </asp:linkButton>
                                </td>
                            </tr>

                            <tr style="height:35px;">
                                <td style="width: 80px;"><asp:Label ID="lab2" Visible="true" Height="30" Text="LABEL2:" runat="server"></asp:Label></td>
                                <td style="text-align:right;"><asp:TextBox ID="label2" Visible="true" ReadOnly="true" placeholder="Optional" onfocus="this.select()" runat="server" Width="200px"></asp:TextBox></td>
                                <td style="padding-left:5px;padding-top:4px;">
                                    <asp:linkButton ID="edit2" Visible="true" runat="server" ForeColor="#737373" OnClick="processRequest" >
                                        <i class="fa fa-pencil-square-o" style="font-size:22px;" BackColor="#e6f3ff" runat="server" ></i>
                                    </asp:linkButton>
                                </td>
                            </tr>

                            <tr style="height:35px;">
                                <td style="width: 80px;"><asp:Label ID="lab3" Visible="true"  Height="30" Text="LABEL3:" runat="server"></asp:Label></td>
                                <td style="text-align:right;"><asp:TextBox ID="label3" Visible="true" ReadOnly="true" placeholder=" Optional" onfocus="this.select()" runat="server" Width="200px"></asp:TextBox></td>
                                <td style="padding-left:5px;padding-top:4px;">
                                     <asp:linkButton ID="edit3" Visible="true" runat="server" ForeColor="#737373" OnClick="processRequest" >
                                        <i class="fa fa-pencil-square-o" style="font-size:22px;" BackColor="#e6f3ff" runat="server" ></i>
                                    </asp:linkButton>
                                </td>
                            </tr>
                            <tr style="height:35px;">
                                <td style="width: 80px;"><asp:Label ID="lab4" Visible="true"  Height="30" Text="LABEL4:" runat="server"></asp:Label></td>
                                <td style="text-align:right;"><asp:TextBox ID="label4" Visible="true" ReadOnly="true" placeholder=" Optional" onfocus="this.select()" runat="server" Width="200px"></asp:TextBox></td>
                                <td style="padding-left:5px;padding-top:4px;">
                                     <asp:linkButton ID="edit4" Visible="true" runat="server" ForeColor="#737373" OnClick="processRequest" >
                                        <i class="fa fa-pencil-square-o" style="font-size:22px;" BackColor="#e6f3ff" runat="server" ></i>
                                    </asp:linkButton>
                                </td>
                            </tr>
                            <tr style="height:35px;">
                                <td style="width:80px;"><asp:Label ID="lab5" Visible="true"  Height="30" Text="LABEL5:" runat="server"></asp:Label></td>
                                <td style="text-align:right;"><asp:TextBox ID="label5" Visible="true" ReadOnly="true" placeholder=" Optional" onfocus="this.select()" runat="server" Width="200px"></asp:TextBox></td>
                                <td style="padding-left:5px;padding-top:4px;">
                                     <asp:linkButton ID="edit5" Visible="true" runat="server" ForeColor="#737373" OnClick="processRequest" >
                                        <i class="fa fa-pencil-square-o" style="font-size:22px;" BackColor="#e6f3ff" runat="server" ></i>
                                    </asp:linkButton>
                                </td>
                            </tr>
                            <tr style="height:35px;">
                                <td style="width:80px;"><asp:Label ID="lab6" Visible="true"  Height="30" Text="LABEL6:" runat="server"></asp:Label></td>
                                <td style="text-align:right;"><asp:TextBox ID="label6" Visible="true" ReadOnly="true" placeholder=" Optional" onfocus="this.select()" runat="server" Width="200px"></asp:TextBox></td>
                                <td style="padding-left:5px;padding-top:4px;">
                                     <asp:linkButton ID="edit6" Visible="true" runat="server" ForeColor="#737373" OnClick="processRequest" >
                                        <i class="fa fa-pencil-square-o" style="font-size:22px;" BackColor="#e6f3ff" runat="server" ></i>
                                    </asp:linkButton>
                                </td>
                            </tr>
                            <tr style="height:35px;">
                                <td style="width:80px;"><asp:Label ID="lab7" Visible="true"  Height="30" Text="LABEL7:" runat="server"></asp:Label></td>
                                <td style="text-align:right;"><asp:TextBox ID="label7" Visible="true" ReadOnly="true" placeholder=" Optional" onfocus="this.select()" runat="server" Width="200px"></asp:TextBox></td>
                                <td style="padding-left:5px;padding-top:4px;">
                                     <asp:linkButton ID="edit7" Visible="true" runat="server" ForeColor="#737373" OnClick="processRequest" >
                                        <i class="fa fa-pencil-square-o" style="font-size:22px;" BackColor="#e6f3ff" runat="server" ></i>
                                    </asp:linkButton>
                                </td>
                            </tr>
                            <tr style="height:35px;">
                                <td style="width:80px;"><asp:Label ID="lab8" Visible="true"  Height="30" Text="LABEL8:" runat="server"></asp:Label></td>
                                <td style="text-align:right;"><asp:TextBox ID="label8" Visible="true" ReadOnly="true" placeholder=" Optional" onfocus="this.select()" runat="server" Width="200px"></asp:TextBox></td>
                                <td style="padding-left:5px;padding-top:4px;">
                                     <asp:linkButton ID="edit8" Visible="true" runat="server" ForeColor="#737373" OnClick="processRequest" >
                                        <i class="fa fa-pencil-square-o" style="font-size:22px;" BackColor="#e6f3ff" runat="server" ></i>
                                    </asp:linkButton>
                                </td>
                            </tr>
                            <tr style="height:35px;">
                                <td style="width:80px;"><asp:Label ID="lab9" Visible="true"  Height="30" Text="LABEL9:" runat="server"></asp:Label></td>
                                <td style="text-align:right;"><asp:TextBox ID="label9" Visible="true" ReadOnly="true" placeholder=" Optional" onfocus="this.select()" runat="server" Width="200px"></asp:TextBox></td>
                                <td style="padding-left:5px;padding-top:4px;">
                                     <asp:linkButton ID="edit9" Visible="true" runat="server" ForeColor="#737373" OnClick="processRequest" >
                                        <i class="fa fa-pencil-square-o" style="font-size:22px;" BackColor="#e6f3ff" runat="server" ></i>
                                    </asp:linkButton>
                                </td>
                            </tr>
                            <tr style="height:35px;">
                                <td style="width:80px;"><asp:Label ID="lab10" Visible="true"  Height="30" Text="LABEL10:" runat="server"></asp:Label></td>
                                <td style="text-align:right;"><asp:TextBox ID="label10" Visible="true" ReadOnly="true" placeholder=" Optional" onfocus="this.select()" runat="server" Width="200px"></asp:TextBox></td>
                                <td style="padding-left:5px;padding-top:4px;">
                                     <asp:linkButton ID="edit10" Visible="true" runat="server" ForeColor="#737373" OnClick="processRequest" >
                                        <i class="fa fa-pencil-square-o" style="font-size:22px;" BackColor="#e6f3ff" runat="server" ></i>
                                    </asp:linkButton>
                                </td>
                            </tr>
                        </table>
                        <div style="height:10px;"><asp:table id="configMsg" runat="server"></asp:table></div>
                        <asp:Panel ID="setupTitle" CssClass="card" style="height:30px;background-color:aliceblue; padding: 0px 5px 0px 10px;width:312px;margin-top:13px;" runat="server">
                            <table style="width:100%;" runat="server">
                                <tr style="height:30px;background-color:aliceblue">
                                    <td style="font-family:Arial;width:190px;white-space:nowrap;font-weight:bold;">Control Setup </td>
                                    <td style="text-align:right;padding-right:5px;">
                                        <asp:linkButton ID="trash" style="padding-left:15px;" Visible="false" runat="server" ForeColor="#737373" OnClick="hideControlInfo_Click" >
                                            <i class="fa fa-trash" style="font-size:18px;" BackColor="#e6f3ff" runat="server" ></i>
                                        </asp:linkButton>
                                        <asp:linkButton ID="hideControlInfo" style="padding-left:15px;" Visible="true" runat="server" ForeColor="#737373" OnClick="hideControlInfo_Click" >
                                            <i class="fa fa-times" style="font-size:20px;" BackColor="#e6f3ff" runat="server" ></i>
                                        </asp:linkButton>
                                    </td>
                                </tr>
                            </table>
                        </asp:Panel>

                        <table id="labelControlsTable" visible="false" style="width: 309px;"  class=auto-style3 runat="server" >
                            <tr>
                                <td style="width:95px"><asp:Label Text="TYPE:" runat="server"></asp:Label></td>
                                <td style="text-align:left;padding:15px 0px 4px 0px;">
                                    <asp:RadioButton ID="textBoxType" Text="&nbsp;Textbox" GroupName="radioGroup" Checked="true" AutoPostBack="true" OnCheckedChanged="radioBtnChanged_Click" runat="server"/>
                                    <asp:RadioButton ID="dropdownType" Text="&nbsp;Dropdown" GroupName="radioGroup" style="margin-left:30px;" AutoPostBack="true" OnCheckedChanged="radioBtnChanged_Click" runat="server"/>
                                </td>
                            </tr>
                           
                            <tr style="vertical-align:top;">
                                <td style="width:75px"><asp:Label Text="NAME:" runat="server"></asp:Label></td>
                                <td style="text-align:right;"><asp:TextBox ID="labelTextBox" placeholder=" Label name" onfocus="this.select()" runat="server" Width="216px"></asp:TextBox></td>
                            </tr>
                             <tr style="vertical-align:top;">
                                <td style="width:75px;padding-top:4px;"><asp:Label ID="valuesLabel" Visible="false" Text="VALUES:" runat="server"></asp:Label></td>
                                <td style="text-align:right;"><asp:TextBox ID="dropdownValues" style="margin-top:7px;" Visible="false" placeholder="Optional.   e.g.: 10, 20, 30, Yes, No" onfocus="this.select()" TextMode="MultiLine" runat="server" Width="216px" Height="70px"></asp:TextBox></td>
                            </tr>
                            <tr style="vertical-align:top;">
                                <td style="width:75px;padding-top:3px;"><asp:Label ID="regex" Text="REGEX:" runat="server"></asp:Label></td>
                                <td style="text-align:right;"><asp:TextBox ID="regexTextBox" style="margin-top:2px;" placeholder="Optional." onfocus="this.select()" TextMode="MultiLine" runat="server" Width="216px" Height="70px"></asp:TextBox></td>
                            </tr>
                            <tr style="vertical-align:top;">
                                <td style="width:75px;padding-top:3px;"><asp:Label ID="alert" Text="ALERT:" runat="server"></asp:Label></td>
                                <td style="text-align:right;"><asp:TextBox ID="msgTextBox" style="margin-top:7px;" placeholder="Message of what a valid entry should be." TextMode="MultiLine" onfocus="this.select()" runat="server" Height="70px" Width="216px"></asp:TextBox></td>
                            </tr>
                            <tr>
                                <td colspan="2" style="text-align:right;padding-top:5px;">
                                    <asp:Button ID="labelContents" CssClass="btn btn-primary" style="padding:1px 10px 0px 10px;" Text="Save" Font-Size="Small" runat="server" OnClick="labelContents_Click" /></td>
                            </tr>
                        </table>
                    </asp:Panel>
                </td>
            </tr>
        </table>
        </div> 
        </ContentTemplate>
    </asp:UpdatePanel>

</asp:Content>
---------Settings.aspx.cs
using System;
using System.Data.SqlClient;
using System.Web.UI;
using System.Web.UI.WebControls;
using System.Data;
using BarcodeConversion.App_Code;
using System.Collections.Generic;
using System.Text.RegularExpressions;
using System.Reflection;
using System.Web;
using System.Linq;

namespace BarcodeConversion
{
    public partial class Contact : Page
    {
        public void Page_Init(object o, EventArgs e)
        {
            //Page.MaintainScrollPositionOnPostBack = true;
        }

        protected void Page_Load(object sender, EventArgs e)
        {
            try
            {
                // Make sure only admins can see Settings page
                if (!Page.IsPostBack) jobAbb.Focus();

                if (userStatus() == "True") SettingsPanel.Visible = true;
                else if (userStatus() == "False")
                {
                    SettingsPanel.Visible = false;
                    Response.Redirect("~/",false);
                    return;
                }
                else if (userStatus() == "Failed") return;
                else if (userStatus() == "Not Found")
                {
                    string msg = "Operator not Found.";
                    ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                    return;
                }
                jobSectionDropdownColor(); // Job section
                setDropdownColor(); // Index Config section
                Page.MaintainScrollPositionOnPostBack = true;
            }
            catch (Exception ex)
            {
                string msg = "Error 50: Issue occured while attempting to load page. Contact system admin.";
                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                // Log the exception and notify system operators
                ExceptionUtility.LogException(ex);
                ExceptionUtility.NotifySystemOps(ex);
                return;
            }
        }



        // CHECK WHETHER USER IS ADMIN
        private string userStatus() 
        {
            string user = HttpContext.Current.User.Identity.Name.Split('\\').Last();
            using (SqlConnection con = Helper.ConnectionObj) 
            {
                using (SqlCommand cmd = con.CreateCommand()) 
                {
                    cmd.CommandText = "SELECT ADMIN FROM OPERATOR WHERE NAME=@userName";
                    cmd.Parameters.AddWithValue("@userName", user);
                    try 
                    {
                        con.Open();
                        object result = cmd.ExecuteScalar();
                        if (result != null)
                            return result.ToString();
                        else return "Not Found";
                    }
                    catch (Exception ex)
                    {
                        string msg = "Error 51: Issue occured while attempting to identify operator status. Contact system admin." ;
                        ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                        // Log the exception and notify system operators
                        ExceptionUtility.LogException(ex);
                        ExceptionUtility.NotifySystemOps(ex);
                        return "Failed";
                    }
                }
            }
        }



        // 'CHOOSE YOUR ACTION' DROPDOWN: CHOOSE TO CREATE/EDIT JOB
        protected void actionChange(object sender, EventArgs e)
        {   
            try
            {
                if (selectAction.SelectedValue == "edit")
                {
                    // Fill dropdown list with jobs.
                    selectJobList.Items.Clear();
                    selectJobList.Items.Add("Select");
                    getDropdownJobItems();
                    jobAbb.Visible = false;
                    createJobBtn.Visible = false;
                    deleteJobBtn.Visible = false;
                    selectJobList.Visible = true;
                    editJobBtn.Visible = true;
                    jobNameLabel.Visible = true;
                    jobName.Visible = true;
                    jobName.Attributes["placeholder"] = " Optional";
                    jobActiveLabel.Visible = true;
                    jobActiveBtn.Visible = true;
                }
                else if (selectAction.SelectedValue == "create")
                {
                    jobSectionDefault();
                }
                else
                {
                    editJobBtn.Visible = false;
                    jobAbb.Visible = false;
                    createJobBtn.Visible = false;
                    jobNameLabel.Visible = false;
                    jobName.Visible = false;
                    jobActiveLabel.Visible = false;
                    jobActiveBtn.Visible = false;
                    jobAssignedToLabel.Visible = false;
                    jobAssignedTo.Visible = false;
                    selectJobList.Visible = true;
                    deleteJobBtn.Visible = true;
                    getDropdownJobItems();
                }
            }
            catch (Exception ex)
            {
                string msg  = "Error 52: Issue occured while attempting to choose action. Contact system admin. " ;
                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                // Log the exception and notify system operators
                ExceptionUtility.LogException(ex);
                ExceptionUtility.NotifySystemOps(ex);
            }
        }



        // 'CREATE' CLICKED: CREATE NEW JOB. FUNCTION
        protected void createJob_Click(object sender, EventArgs e)
        {   
            try
            {
                if (!Page.IsValid) return;
                if (this.jobAbb.Text == string.Empty)
                {
                    string msg = "Job Abbreviation required.";
                    onScreenMsg(msg, "#ff3333;", "jobSetupSection");
                    jobAbb.Text = string.Empty;
                    jobAbb.Focus();
                    return;
                }
                else if (jobAbb.Text.Length > 6)
                {
                    string msg = "6 characters max Job Abbreviation.";
                    onScreenMsg(msg, "#ff3333;", "jobSetupSection");
                    jobAbb.Text = string.Empty;
                    jobAbb.Focus();
                    return;
                }
                else if (this.jobName.Text == string.Empty)
                {
                    string msg = "Job Name required.";
                    onScreenMsg(msg, "#ff3333;", "jobSetupSection");
                    jobName.Text = string.Empty;
                    jobName.Focus();
                    return;
                }
                else
                {
                    try
                    {
                        using(SqlConnection con = Helper.ConnectionObj) 
                        {
                            // Save created job
                            using (SqlCommand cmd = con.CreateCommand()) 
                            {
                                cmd.CommandText = "INSERT INTO JOB (ABBREVIATION, NAME, ACTIVE) VALUES(@abbr, @name, @active)";
                                cmd.Parameters.AddWithValue("@abbr", this.jobAbb.Text);
                                cmd.Parameters.AddWithValue("@name", this.jobName.Text);
                                cmd.Parameters.AddWithValue("@active", this.jobActiveBtn.SelectedValue.ToUpper());
                                con.Open();
                                if (cmd.ExecuteNonQuery() == 1)
                                {
                                    // Assign job to assignee
                                    if (jobAssignedTo.SelectedValue != "Select")
                                    {
                                        string assignee = jobAssignedTo.SelectedValue;
                                        string abbr = jobAbb.Text;
                                        bool answer = AssignJob(assignee, abbr); // calling assignJob function
                                        if (answer == true)
                                        {
                                            string msg = "New job saved & assigned.";
                                            onScreenMsg(msg, "green;", "jobSetupSection");
                                            ScriptManager.RegisterStartupScript(Page, Page.GetType(), "fadeoutOp", "FadeOut4();", true);
                                            jobFormClear();
                                            object b = this.Master.FindControl("MainContent").FindControl("unassignedBtn") as Button;
                                            getUnassignedJobs(b);
                                        }
                                        else
                                        {
                                            string msg = "Error 53: New job successfully saved, but not assigned! Contact system admin.";
                                            ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                                            jobFormClear();
                                        }
                                    }
                                    else
                                    {
                                        string msg = "New job created.";
                                        onScreenMsg(msg, "green;", "jobSetupSection");
                                        ScriptManager.RegisterStartupScript(Page, Page.GetType(), "fadeoutOp", "FadeOut4();", true);
                                        jobFormClear();
                                        object b = this.Master.FindControl("MainContent").FindControl("unassignedBtn") as Button;
                                        getUnassignedJobs(b);
                                    }
                                }
                            }
                        }
                    }
                    catch (Exception ex)
                    {
                        jobFormClear();
                        if (ex.Message.Contains("Violation of UNIQUE KEY"))
                        {
                            string msg = "Job Abbreviation already exists.";
                            onScreenMsg(msg, "#ff3333;", "jobSetupSection");
                        }
                        else
                        {
                            string msg = "Error 54: Issue occured while attempting to save the created job. Contact system admin.";
                            ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                            // Log the exception and notify system operators
                            ExceptionUtility.LogException(ex);
                            ExceptionUtility.NotifySystemOps(ex);
                        }
                    }
                }
            }
            catch (Exception ex)
            {
                string msg  = "Error 55: Issue occured while attempting to save the created job. Contact system admin." ;
                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                // Log the exception and notify system operators
                ExceptionUtility.LogException(ex);
                ExceptionUtility.NotifySystemOps(ex);
            }
        }

        
        // 'EDIT' CLICKED: EDIT JOB NAME OR ACTIVE STATUS. FUNCTION
        protected void editJob_Click(object sender, EventArgs e)
        {
            if (!Page.IsValid) return;
            try
            {
                // Edit job ACTIVE status
                using (SqlConnection con = Helper.ConnectionObj)
                {
                    using (SqlCommand cmd = con.CreateCommand())
                    {
                        if (this.selectJobList.SelectedValue == "Select")
                        {
                            string msg = "Please select a Job to edit.";
                            onScreenMsg(msg, "#ff3333;", "jobSetupSection");
                            jobFormClear();
                            return;
                        }
                        if (this.jobName.Text == string.Empty)
                            cmd.CommandText = "UPDATE JOB SET ACTIVE=@active WHERE ABBREVIATION=@abbr";
                        else
                        {
                            cmd.CommandText = "UPDATE JOB SET NAME=@job, ACTIVE=@active WHERE ABBREVIATION=@abbr";
                            cmd.Parameters.AddWithValue("@job", this.jobName.Text);
                        }
                        cmd.Parameters.AddWithValue("@active", this.jobActiveBtn.SelectedValue);
                        cmd.Parameters.AddWithValue("@abbr", this.selectJobList.SelectedValue);
                        con.Open();
                        if (cmd.ExecuteNonQuery() == 1)
                        {
                            // Assign job to assignee
                            if (jobAssignedTo.Visible = true && jobAssignedTo.SelectedValue != "Select")
                            {
                                string assignee = jobAssignedTo.SelectedValue;
                                string abbr = this.selectJobList.SelectedValue;
                                bool answer = AssignJob(assignee, abbr); // calling assignJob function
                                if (answer == true)
                                {
                                    string msg = "Job updated & assigned.";
                                    onScreenMsg(msg, "green;", "jobSetupSection");
                                    ScriptManager.RegisterStartupScript(Page, Page.GetType(), "fadeoutOp", "FadeOut4();", true);
                                    jobFormClear();
                                }
                                else
                                {
                                    string msg = "Error 55a: Job updated successfully, but not assigned! Contact System Admin.";
                                    ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                                    jobFormClear();
                                }
                            }
                            else
                            {
                                string msg = "Job updated successfully.";
                                onScreenMsg(msg, "green;", "jobSetupSection");
                                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "fadeoutOp", "FadeOut4();", true);
                                jobFormClear();
                            }
                            getDropdownJobItems();
                            getActiveJobs();
                            getUnassignedJobs(null);
                        }
                    }
                } 
            }
            catch (Exception ex)
            {
                string msg = "Error 56: Issue occured while attempting to update the job ACTIVE status. Contact system admin.";
                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                // Log the exception and notify system operators
                ExceptionUtility.LogException(ex);
                ExceptionUtility.NotifySystemOps(ex);
            }
        }
        


        // 'DELETE' CLICKED: DELETE JOB. FUNCTION
        protected void deleteJob_Click(object sender, EventArgs e) { }

    

        // 'SUBMIT' CLICKED: SET OPERATOR ADMIN PERMISSIONS. FUNCTION
        protected void setPermissions_Click(object sender, EventArgs e)
        {
            if (!Page.IsValid) return;
            try
            {
                if (user.Text != string.Empty)
                {
                    string msg;
                    // If user exists, get ID
                    int opId = Helper.getUserId(user.Text);

                    // If user exists, set Admin status
                    using (SqlConnection con = Helper.ConnectionObj)
                    {
                        using (SqlCommand cmd = con.CreateCommand())
                        {
                            cmd.CommandText = "UPDATE OPERATOR SET ADMIN=@admin WHERE NAME=@user";
                            cmd.Parameters.AddWithValue("@admin", this.permissions.SelectedValue);
                            cmd.Parameters.AddWithValue("@user", this.user.Text);
                            con.Open();
                            if (cmd.ExecuteNonQuery() == 1)
                            {
                                msg = "Permissions set.";
                                onScreenMsg(msg, "green;", "userSection");
                                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "fadeoutOp", "FadeOut2();", true);
                                getOperators(); // Get operators

                                // If operator becomes Admin, remove assigned jobs in OPERATOR_ACCESS
                                if (permissions.SelectedValue == "1")
                                {
                                    //cmd.Parameters.Clear();
                                    //cmd.CommandText = "DELETE FROM OPERATOR_ACCESS WHERE OPERATOR_ID=@opID";
                                    //cmd.Parameters.AddWithValue("@opID", opId);
                                    //cmd.ExecuteNonQuery();
                                }
                                // If admin becomes Operator
                                else
                                {   
                                    // If self demotion, hide Settings & redirect to Home page.
                                    string op = HttpContext.Current.User.Identity.Name.Split('\\').Last();
                                    if (op == user.Text)
                                    {
                                        LinkButton l = this.Master.FindControl("settings") as LinkButton;
                                        l.Visible = false;
                                        Response.Redirect("~/",false);
                                    }
                                }
                                permissionsFormClear();
                                return;
                            }

                            // If user doesn't exist, register user and set Admin status.
                            cmd.CommandText = "INSERT INTO OPERATOR (NAME, ADMIN) VALUES(@user,@admin)";
                            if (cmd.ExecuteNonQuery() == 1)
                            {
                                msg = "New operator added.";
                                onScreenMsg(msg, "green;", "userSection");
                                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "fadeoutOp", "FadeOut2();", true);
                                permissionsFormClear();
                            }
                        }
                    }
                }
                else
                {
                    string msg = "Operator field is required";
                    onScreenMsg(msg, "#ff3333;", "userSection");
                    permissionsFormClear();
                }
                getOperators(); // Get operators
            }
            catch (Exception ex)
            {   
                string msg  = "Error 58: Issue occured while attempting to set permissions. Contact system admin.";
                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                // Log the exception and notify system operators
                ExceptionUtility.LogException(ex);
                ExceptionUtility.NotifySystemOps(ex);
            }
        }



        // CHECKBOX THAT SETS ALL THE OTHERS. HELPER FUNCTION
        protected void selectAll_changed(object sender, EventArgs e)
        {   
            try
            {
                CheckBox ChkBoxHeader = (CheckBox)jobAccessGridView.HeaderRow.FindControl("selectAll");
                foreach (GridViewRow row in jobAccessGridView.Rows)
                {
                    CheckBox ChkBoxRows = (CheckBox)row.FindControl("cbSelect");
                    if (ChkBoxHeader.Checked == true)
                        ChkBoxRows.Checked = true;
                    else ChkBoxRows.Checked = false;
                }
            }
            catch (Exception ex)
            {   
                string msg  = "Error 59: Issue occured while processing master CheckBox. Contact system admin.";
                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                // Log the exception and notify system operators
                ExceptionUtility.LogException(ex);
                ExceptionUtility.NotifySystemOps(ex);
            }
        }



        // CLEAR JOB FORM. HELPER FUNCTION
        private void jobFormClear()
        {   
            try
            {
                jobAbb.Text = string.Empty;
                selectJobList.SelectedValue = "Select";
                jobName.Text = string.Empty;
                jobActiveBtn.SelectedValue = "1";
                jobAssignedTo.SelectedValue = "Select";
                jobAssignedToLabel.Visible = true;
                jobAssignedTo.Visible = true;
                getOperators();
                jobAbb.Focus();
            }
            catch (Exception ex)
            {   
                string msg  = "Error 60: Issue occured while attempting to clear fields. Contact system admin.";
                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                // Log the exception and notify system operators
                ExceptionUtility.LogException(ex);
                ExceptionUtility.NotifySystemOps(ex);
            }
        }



        // CLEAR PERMISSIONS FORM. HELPER FUNCTION
        private void permissionsFormClear()
        {
            user.Text = string.Empty;
            permissions.SelectedValue = "0";
            user.Focus();
        }



        // HIDE/COLLAPSE JOB SECTION. FUNCTION
        protected void newJobShow_Click(object sender, EventArgs e)
        {
           try
           {
                if (jobSection.Visible == false)
                {
                    jobSection.Visible = true;
                    line.Visible = true;
                    jobFormClear();
                    jobSectionDefault();
                }
                else
                {
                    jobSection.Visible = false;
                    if (newUserSection.Visible == false) line.Visible = false;
                }
            }
           catch (Exception ex)
           {
                string msg  = "Error 61: Issue occured while attempting to hide or collapse panel. Contact system admin." ;
                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                // Log the exception and notify system operators
                ExceptionUtility.LogException(ex);
                ExceptionUtility.NotifySystemOps(ex);
            }
        }



        // 'PERMISSION SECTION' CLICKED: HIDE/SHOW USER PERMISSION SECTION
        protected void permissionsShow_Click(object sender, EventArgs e)
        {
            if(newUserSection.Visible == false)
            {
                newUserSection.Visible = true;
                line.Visible = true;
            }
            else
            {
                newUserSection.Visible = false;
                if (jobSection.Visible == false) line.Visible = false;
            }          
        }



        // 'JOB ACCESS SECTION' CLICKED: HIDE/SHOW JOB-ACCESS SECTION & PULL InnaccessibleED JOBS
        protected void assignShow_Click(object sender, EventArgs e)
        {   
            try
            {
                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "scroll", "scrollToBottom();", true);     // Scroll page all the way down

                if (assignPanel.Visible == false)
                {
                    Page.Validate();
                    assignPanel.Visible = true;
                    assignee.SelectedValue = "Select";
                    // Get all unassigned jobs
                    getUnassignedJobs(null);
                    getOperators();
                }
                else
                {
                    assignPanel.Visible = false;
                }
            }
            catch (Exception ex)
            {
                string msg  = "Error 62: Issue occured while attempting to hide or show panel. Contact system admin.";
                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                // Log the exception and notify system operators
                ExceptionUtility.LogException(ex);
                ExceptionUtility.NotifySystemOps(ex);
            }
        }



        // 'OPERATOR' SELECTION
        private void getOperators()
        {
            try
            {
                assignee.Items.Clear();
                jobAssignedTo.Items.Clear();
                assignee.Items.Add("Select");
                jobAssignedTo.Items.Add("Select");
                using (SqlConnection con = Helper.ConnectionObj)
                {
                    using (SqlCommand cmd = con.CreateCommand())
                    {
                        cmd.CommandText = "SELECT NAME FROM OPERATOR WHERE ADMIN=0";
                        con.Open();
                        using (SqlDataReader reader = cmd.ExecuteReader())
                        {
                            if (reader.HasRows)
                            {
                                while (reader.Read())
                                {   
                                    assignee.Items.Add(reader.GetValue(0).ToString());
                                    jobAssignedTo.Items.Add(reader.GetValue(0).ToString());
                                }
                            }
                        }
                    }
                }
            }
            catch(Exception ex)
            {
                string msg = "Error 62a: Issue occured while retrieving operators. Contact system admin.";
                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                // Log the exception and notify system operators
                ExceptionUtility.LogException(ex);
                ExceptionUtility.NotifySystemOps(ex);
            }
        }



        // 'OPERATOR' SELECTED (JOB ACCESS SECTION)
        protected void getInaccessibleJobs(object sender, EventArgs e)
        {   
            try
            {
                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "scroll", "scrollToBottom();", true);     // Scroll page all the way down

                if (assignee.SelectedValue != "Select")
                {
                    jobAccessGridView.PageIndex = 0;
                    object b = this.Master.FindControl("MainContent").FindControl("inaccessibleBtn") as Button;
                    if (b != null) getUnassignedJobs(b);
                }
                else getUnassignedJobs(null);
            }
            catch(Exception ex)
            {
                string msg = "Error 63: Issue occured while retrieving inaccessible jobs. Contact system admin.";
                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                // Log the exception and notify system operators
                ExceptionUtility.LogException(ex);
                ExceptionUtility.NotifySystemOps(ex);
            }
        }


        // 'ACCESSIBLE' CLICKED: GET OP'S ACCESSIBLE JOBS. FUNCTION
        protected void assignedJob_Click(object sender, EventArgs e)
        {
            ScriptManager.RegisterStartupScript(Page, Page.GetType(), "scroll", "scrollToBottom();", true);     // Scroll page all the way down
            jobAccessGridView.PageIndex = 0;
            getAssignedJobs();
        }



        // 'ACCESSIBLE' CLICKED: GET OP'S ACCESSIBLE JOBS. HELPER FUNCTION
        private void getAssignedJobs()
        {
            try
            {
                if (assignee.SelectedValue == "Select")
                {
                    onScreenMsg("Required", "#ff3333;", "jobAccessSection1");
                    return;
                }
                
                // If operator field not empty, get operator ID, then jobs
                int opID = 0;
                using (SqlConnection con = Helper.ConnectionObj)
                {
                    using (SqlCommand cmd = con.CreateCommand())
                    {
                        cmd.CommandText = "SELECT ID FROM OPERATOR WHERE NAME=@name";
                        cmd.Parameters.AddWithValue("@name", assignee.SelectedValue);
                        con.Open();
                        object result = cmd.ExecuteScalar();
                        if (result != null) opID = (int)result;
                        else
                        {
                            string msg = "Specified Operator could not be found!";
                            ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                            assignee.Focus();
                            return;
                        }
                        jobsLabel.Text = "Accessible Jobs";

                        // Get operator's assigned jobs
                        jobAccessBtn.Visible = false;
                        cmd.Parameters.Clear();
                        cmd.CommandText =   "SELECT ABBREVIATION " +
                                            "FROM JOB " +
                                            "INNER JOIN OPERATOR_ACCESS ON JOB.ID=OPERATOR_ACCESS.JOB_ID " +
                                            "WHERE ACTIVE=1 AND OPERATOR_ACCESS.OPERATOR_ID=@ID " +
                                            "ORDER BY ABBREVIATION ASC";
                        cmd.Parameters.AddWithValue("@ID", opID);
                        using (SqlDataAdapter da = new SqlDataAdapter(cmd))
                        {
                            DataSet ds = new DataSet();
                            da.Fill(ds);
                            if (ds.Tables.Count > 0)
                            {
                                jobAccessGridView.DataSource = ds.Tables[0];
                                jobAccessGridView.DataBind();
                                jobAccessGridView.Visible = true;
                            }

                            // Handling of whether any JOB was returned from DB
                            if (jobAccessGridView.Rows.Count == 0)
                            {
                                jobAccessGridView.Visible = false;
                                jobAccessBtn.Visible = false;
                                deleteAssignedBtn.Visible = false;
                                jobsLabel.Text = "No Accessible Jobs";
                            }
                            else
                            {
                                jobAccessBtn.Visible = false;
                                deleteAssignedBtn.Visible = true;
                            }
                        }
                    }
                }
            }
            catch (Exception ex)
            {
                string msg  = "Error 65: Issue occured while attempting to get operator's accessible jobs. Contact system admin.";
                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                // Log the exception and notify system operators
                ExceptionUtility.LogException(ex);
                ExceptionUtility.NotifySystemOps(ex);
            }
        }
        


        // 'ACTIVE' CLICKED: GET ALL ACTIVE JOBS. FUNCTION
        protected void unassignedJob_Click(object sender, EventArgs e)
        {   
            try
            {
                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "scroll", "scrollToBottom();", true);     // Scroll page all the way down
                jobAccessGridView.PageIndex = 0;
                getUnassignedJobs(sender);
            }
            catch (Exception ex)
            {
                string msg  = "Error 66: Issue occured while attempting to retrieve active jobs. Contact system admin.";
                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                // Log the exception and notify system operators
                ExceptionUtility.LogException(ex);
                ExceptionUtility.NotifySystemOps(ex);
            }
        }



        // 'DENY' CLICKED: REMOVE OPERATOR ACCESS TO JOBS. FUNCTION
        protected void deleteAssigned_Click(object sender, EventArgs e)
        {   
            try
            {
                int opID = 0, jobID = 0, count = 0;
                if (assignee.SelectedValue == "Select")
                {
                    string msg = "Operator field is required!";
                    ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                    assignee.Focus();
                    return;
                }
                using (SqlConnection con = Helper.ConnectionObj)
                {
                    using (SqlCommand cmd = con.CreateCommand())
                    {
                        // If operator field not empty, get Operator ID
                        cmd.CommandText = "SELECT ID FROM OPERATOR WHERE NAME = @name";
                        cmd.Parameters.AddWithValue("@name", assignee.SelectedValue);
                        con.Open();
                        object result = cmd.ExecuteScalar();
                        if (result != null) opID = (int)result;
                        else
                        {
                            string msg = "Specified Operator could not be found!";
                            ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                            assignee.Focus();
                            assignedJob_Click(new object(), new EventArgs());
                            return;
                        }

                        // For each checked job, remove access.
                        foreach (GridViewRow row in jobAccessGridView.Rows)
                        {
                            CheckBox chxBox = row.FindControl("cbSelect") as CheckBox;
                            if (chxBox.Checked)
                            {
                                // First, get ID of selectd job
                                cmd.Parameters.Clear();
                                cmd.CommandText = "SELECT ID FROM JOB WHERE ABBREVIATION = @abbr";
                                cmd.Parameters.AddWithValue("@abbr", row.Cells[2].Text);
                                object result2 = cmd.ExecuteScalar();
                                if (result2 != null) jobID = (int)result2;
                                else
                                {
                                    string msg = "Error 68: Selected job " + row.Cells[2].Text + " could not be found. Contact system admin.";
                                    ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                                    assignee.Focus();
                                    assignedJob_Click(new object(), new EventArgs());
                                    return;
                                }

                                // Then, remove job from OPERATOR_ACCESS
                                cmd.Parameters.Clear();
                                cmd.CommandText = "DELETE FROM OPERATOR_ACCESS WHERE OPERATOR_ACCESS.JOB_ID = @job AND OPERATOR_ACCESS.OPERATOR_ID = @op";
                                cmd.Parameters.AddWithValue("@job", jobID);
                                cmd.Parameters.AddWithValue("@op", opID);
                                if (cmd.ExecuteNonQuery() == 1) count++;
                                else
                                {
                                    string msg = "Error 70: Something went wrong while removing job: " + row.Cells[2].Text + ". Contact system admin.";
                                    ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                                    assignedJob_Click(new object(), new EventArgs());
                                    assignee.Focus();
                                }
                            }
                        }

                        // Handling whether or not any job access was denied
                        if (count == 0)
                        {
                            onScreenMsg("Select at least 1 job","#ff3333;","jobAccessSection2");
                            assignedJob_Click(new object(), new EventArgs());
                        }
                        else
                        {
                            string msg = count + " Job(s) denied";
                            onScreenMsg(msg, "green;", "jobAccessSection2");
                            ScriptManager.RegisterStartupScript(Page, Page.GetType(), "fadeoutOp", "FadeOut();", true);
                            assignedJob_Click(new object(), new EventArgs());
                        }
                    }
                }
            }
            catch (Exception ex)
            {
                string msg = "Error 71: Issue occured while attempting to deny operator's job accesses. Contact system admin.";
                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                // Log the exception and notify system operators
                ExceptionUtility.LogException(ex);
                ExceptionUtility.NotifySystemOps(ex);
            }
        }



        // 'GRANT' CLICKED: ASSIGN OPERATORS JOB-ACCESSES. FUNCTION
        protected void jobAccess_Click(object sender, EventArgs e)
        {   
            try
            {   
                string assigneeName = assignee.SelectedValue;
                int countGranted = 0, countChecked = 0, countError = 0;
                if (assigneeName == "Select")
                {
                    onScreenMsg("Required", "#ff3333;", "jobAccessSection1");
                    assignee.Focus();
                    return;
                }
                
                if (jobAccessGridView.Rows.Count > 0)
                {
                    // For each checked job, assign job to assignee
                    foreach (GridViewRow row in jobAccessGridView.Rows)
                    {
                        CheckBox chxBox = row.FindControl("cbSelect") as CheckBox;
                        if (chxBox.Checked)
                        {
                            countChecked++;
                            string abbr = row.Cells[2].Text;
                            bool answer = AssignJob(assigneeName, abbr); // calling assignJob function
                            if (answer == true) countGranted++;
                            else
                            {
                                chxBox.Checked = false;
                                countError++;
                            }
                        }
                    }

                    // Handling whether any job access was granted.
                    if (countChecked == 0)
                    {
                        onScreenMsg("Select at least 1 job", "#ff3333;", "jobAccessSection2");
                        return;
                    }
                    else
                    {
                        jobAccessGridView.PageIndex = 0;
                        if (countGranted > 0 && countGranted == countChecked)
                        {
                            string msg = countGranted + " job(s) granted";
                            onScreenMsg(msg, "green;", "jobAccessSection2");
                            ScriptManager.RegisterStartupScript(Page,Page.GetType(), "fadeoutOp", "FadeOut();", true);
                            getUnassignedJobs(sender);
                            return;
                        }
                        else // No longer in effect...
                        {
                            string msg = countGranted + " Job(s) granted. Operator already has access to " +countError+ " Job(s)";
                            ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                            getUnassignedJobs(sender);
                            assignee.Focus();
                            return;
                        }
                    }
                }
                else
                {
                    string msg = "There are no jobs to be granted access to.";
                    ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                    return;
                }
            }
            catch (Exception ex)
            {
                string msg  = "Error 72: Issue occured while attempting to grant jobs accesses to operator. Contact system admin.";
                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                // Log the exception and notify system operators
                ExceptionUtility.LogException(ex);
                ExceptionUtility.NotifySystemOps(ex);
            }
        }



        // 'JOB INDEX CONFIGURATION SECTION' CLICKED: SHOW & INDEX FORM CONTROLS. FUNCTION
        protected void jobIndexEditingShow_Click(object sender, EventArgs e)
        {   
            try
            {
                // Fill Job Abb. dropdown list with jobs.
                selectJob.Items.Clear();
                selectJob.Items.Add("Select");
                if (jobIndexEditingPanel.Visible == false)
                {
                    jobIndexEditingPanel.Visible = true;
                    labelsTable.Visible = false;
                    labelControlsTable.Visible = false;
                    setupTitle.Visible = false;
                    getActiveJobs();
                }
                else
                {
                    jobIndexEditingPanel.Visible = false;
                }
            }
            catch (Exception ex)
            {
                string msg  = "Error 73: Issue occured while attempting to show or hide section. Contact system admin.";
                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                // Log the exception and notify system operators
                ExceptionUtility.LogException(ex);
                ExceptionUtility.NotifySystemOps(ex);
            }
        }



        // 'JOB ABBREVIATION' DROPDOWN SELECT
        protected void JobAbbSelect(object sender, EventArgs e)
        {   
            try
            {
                if (selectJob.SelectedValue != "Select")
                {
                    ScriptManager.RegisterStartupScript(Page, Page.GetType(), "scroll", "scrollToBottom();", true);     // Scroll page all the way down 
                    labelsTable.Visible = true;
                    edit1.Visible = false;
                    for (int i=2; i<=10; i++)
                    {
                        LinkButton l = this.Master.FindControl("MainContent").FindControl("edit" + i) as LinkButton;
                        l.Visible = true;
                    }
                    labelControlsTable.Visible = true;
                    setupTitle.Visible = true;

                    // Clear labelControlsTable current values
                    labelTextBox.Text = string.Empty;
                    dropdownValues.Text = string.Empty;
                    regexTextBox.Text = string.Empty;
                    msgTextBox.Text = string.Empty;

                    // Get type of 1st control 
                    int jobId = Helper.getJobId(selectJob.SelectedValue);
                    var controlInfo = isDropdownType(1, jobId);
                    
                    bool isLabel1Set = controlInfo.Item1;
                    bool isControlDropdown = controlInfo.Item2;

                    if (isControlDropdown == true)
                    {
                        dropdownType.Checked = true;
                        textBoxType.Checked = false;
                        dropdownValues.Visible = true;
                        valuesLabel.Visible = true;
                        regexTextBox.Visible = false;
                        msgTextBox.Visible = false;
                        regex.Visible = false;
                        alert.Visible = false;

                        string values = getDropdownValues(jobId, 1);
                        dropdownValues.Text = values;
                    }
                    else
                    {
                        textBoxType.Checked = true;
                        dropdownType.Checked = false;
                        dropdownValues.Visible = false;
                        valuesLabel.Visible = false;
                        regexTextBox.Visible = true;
                        msgTextBox.Visible = true;
                        regex.Visible = true;
                        alert.Visible = true;
                    }

                    // Retrieve & fill controls textboxes
                    for (int i = 1; i <= 10; i++)
                    {
                        using (SqlConnection con = Helper.ConnectionObj)
                        {
                            using (SqlCommand cmd = con.CreateCommand())
                            {
                                con.Open();
                                TextBox t = this.Master.FindControl("MainContent").FindControl("LABEL" + i) as TextBox;
                                cmd.CommandText = "SELECT LABEL" + i + " FROM JOB_CONFIG_INDEX WHERE JOB_ID=@jobID";
                                cmd.Parameters.AddWithValue("@jobID", jobId);
                                object result = cmd.ExecuteScalar();
                                if (result != null)
                                {
                                    if (result.ToString() != DBNull.Value.ToString())
                                    {
                                        t.Text = " " + (string)result;
                                        if (i == 1)
                                        {
                                            processRequest(new object(), new EventArgs());
                                            labelTextBox.Text = " " + (string)result;
                                            isLabel1Set = true;
                                        }
                                    }
                                    else
                                    {
                                        if (i == 1) t.Text = " Required";
                                        else t.Text = " Optional";
                                        if (isLabel1Set == false)
                                            labelTextBox.Attributes["placeholder"] = " Required";
                                    }
                                }
                                else
                                {
                                    if (i == 1) t.Text = " Required";
                                    else t.Text = " Optional";
                                    if (isLabel1Set == false)
                                        labelTextBox.Attributes["placeholder"] = " Required";
                                }
                            }
                        }
                    }
                    labelTextBox.Focus();
                }
                else
                {
                    labelsTable.Visible = false;
                    labelControlsTable.Visible = false;
                    setupTitle.Visible = false;
                }
            }
            catch(Exception ex)
            {
                string msg = "Error 73a: Issue occured while retrieving saved index data. Contact system admin.";
                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                // Log the exception and notify system operators
                ExceptionUtility.LogException(ex);
                ExceptionUtility.NotifySystemOps(ex);
            }
            
        }


        // SET COLOR FOR DROPDOWN CONFIGURED JOB ITEMS. FUNCTION
        protected void onJobSelect(object sender, EventArgs e)
        {
            //setDropdownColor();
        }



        // 'ACTIVE' SELECTED: SET 'ASSIGN TO' VISIBLE OR NOT
        protected void onActiveSelect(object sender, EventArgs e)
        {
            if(jobActiveBtn.Visible && jobActiveBtn.SelectedValue == "1")
            {
                jobAssignedToLabel.Visible = true;
                jobAssignedTo.Visible = true;
                getOperators();
            }
            else
            {
                jobAssignedToLabel.Visible = false;
                jobAssignedTo.Visible = false;
            }
        }
        

        // 'X' ICON CLICKED: CLOSE CONTROL INFOs TABLE
        protected void hideControlInfo_Click(object sender, EventArgs e)
        {
            setDropdownColor();
            labelControlsTable.Visible = false;
            setupTitle.Visible = false;
            for (int i = 1; i <= 10; i++)
            {
                LinkButton btn = this.Master.FindControl("MainContent").FindControl("edit" + i) as LinkButton;
                TextBox t = this.Master.FindControl("MainContent").FindControl("label" + i) as TextBox;
                if (btn.Visible == false)
                {
                    btn.Visible = true;
                }
            }
        }


        // 'SAVE' CLICKED: ADD LABEL CONTROLS
        protected void labelContents_Click(object sender, EventArgs e)
        {
            try
            {
                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "scroll", "scrollToBottom();", true);     // Scroll page all the way down

                if (labelTextBox.Text != string.Empty)
                {
                    // Make sure that regex & message fields are both either filled or blank
                    if ((regexTextBox.Text == string.Empty && msgTextBox.Text != string.Empty) || (regexTextBox.Text != string.Empty && msgTextBox.Text == string.Empty))
                    {
                        string msg = "Both regex and alert must be set or left blank.";
                        onScreenMsg(msg, "#ff3333;", "configSection");
                        labelTextBox.Attributes["placeholder"] = " Required";
                        if (regexTextBox.Text == string.Empty) regexTextBox.Focus();
                        else if (msgTextBox.Text == string.Empty) msgTextBox.Focus();
                        return;
                    }

                    // If Regex entered, check whether it's valid
                    if (regexTextBox.Text != string.Empty)
                    {
                        bool isValid = IsValidRegex(regexTextBox.Text.Trim());
                        if (isValid == false)
                        {
                            string msg = "Invalid regex! You can test it at regexr.com";
                            onScreenMsg(msg, "#ff3333;", "configSection");
                            regexTextBox.Focus();
                            return;
                        }
                    }
                }

                // Get job id
                int jobId = Helper.getJobId(selectJob.SelectedValue);
                if (jobId <= 0)
                {
                    string msg = "Job selected could not be found.";
                    ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                    selectJob.SelectedValue = "Select";
                    return;
                }

                // Check if job already configured
                var controlInfo = isDropdownType(1, jobId);
                bool isJobConfigured = controlInfo.Item1;
                bool isControlDropdown = controlInfo.Item2;

                // Get current control
                int controlBeingSet = 0;
                for (int i = 1; i <= 10; i++)
                {
                    LinkButton btn = this.Master.FindControl("MainContent").FindControl("edit" + i) as LinkButton;
                    if (btn.Visible == false)
                    {
                        controlBeingSet = i;
                    }
                }

                // Make sure 1st control NAME field isn't blank
                if (controlBeingSet == 1)
                {
                    if (labelTextBox.Text == string.Empty)
                    {
                        string msg = "NAME field required.";
                        onScreenMsg(msg, "#ff3333;", "configSection");
                        labelTextBox.Focus();
                        return;
                    }
                }

                // If job not configured
                if (!isJobConfigured)
                {
                    if (dropdownValues.Visible)
                    {
                        if (dropdownValues.Text == string.Empty)
                        {
                            // Just Save job config in JOB_CONFIG_INDEX
                            int result = saveJobConfig(jobId, 1, "dropdown", "insert");
                            handleResult(result, "B");
                            return;
                        }
                        else    // if dropdownValues = not empty
                        {
                            string[] values = dropdownValues.Text.Split(',');

                            // First Save values in INDEX_TABLE_FIELD 
                            for (int i = 1; i <= values.Length; i++)
                            {
                                int result2 = saveDropdownValues(jobId, values[i - 1].Trim(), controlBeingSet, i);
                                if (result2 <= 0)
                                {
                                    string msg = "Error 73C. Could not save dropdown values";
                                    ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                                    selectJob.SelectedValue = "Select";
                                    return;
                                }
                            }

                            // Then Save job config in JOB_CONFIG_INDEX
                            int result = saveJobConfig(jobId, 1, "dropdown", "insert");
                            handleResult(result, "D");
                            return;
                        }
                    }
                    else // If control type = textbox
                    {
                        int result = saveJobConfig(jobId, 1, "textbox", "insert");
                        handleResult(result, "E");
                        return;
                    }
                }
                else    // If job already configured
                {
                    if (dropdownValues.Visible)
                    {
                        // First, delete current dropdown values from INDEX_TABLE_FIELD
                        deleteDropdownValues(jobId, controlBeingSet);

                        if (labelTextBox.Text == string.Empty || dropdownValues.Text == string.Empty)
                        {   
                            // Then update job config
                            int result2 = saveJobConfig(jobId, controlBeingSet, "dropdown", "update");
                            handleResult(result2, "F");
                            return;
                        }
                        string[] values = dropdownValues.Text.Split(',');

                        // Save values in INDEX_TABLE_FIELD first
                        for (int i = 1; i <= values.Length; i++)
                        {
                            int result2 = saveDropdownValues(jobId, values[i - 1].Trim(), controlBeingSet, i);
                            if (result2 <= 0)
                            {
                                string msg = "Error 73G. Could not save dropdown values";
                                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                                selectJob.SelectedValue = "Select";
                                return;
                            }
                        }

                        // Then Save job config in JOB_CONFIG_INDEX
                        int result = saveJobConfig(jobId, controlBeingSet, "dropdown", "update");
                        handleResult(result, "H");
                        return;
                    }
                    else // If control type = textbox
                    {
                        if (isControlDropdown) deleteDropdownValues(jobId, controlBeingSet);
                        int result = saveJobConfig(jobId, controlBeingSet, "textbox", "update");
                        handleResult(result, "I");
                        return;
                    }
                }
            }
            catch (Exception ex)
            {
                string msg = "Error 74: Issue occured while attempting to add label controls. Contact system admin.";
                if (ex.Message.Contains("Cannot insert the value NULL"))
                {
                    msg = " Label1 is required! Please make sure NAME field is not blank while setting Label1.";
                    labelControlsTable.Visible = false;
                    setupTitle.Visible = false;
                    edit1.Visible = true;
                }
                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                // Log the exception and notify system operators
                ExceptionUtility.LogException(ex);
                ExceptionUtility.NotifySystemOps(ex);
            }
        }



        // REGEX VALIDATOR FUNCTION
        private static bool IsValidRegex(string pattern)
        {
            if (string.IsNullOrEmpty(pattern)) return false;

            try
            {
                Regex.Match("", pattern);
            }
            catch (ArgumentException)
            {
                return false;
            }
            return true;
        }


        // 'EDIT' ICON CLICKED: EDIT LABEL CONTENTS
        protected void processRequest(object sender, EventArgs e)
        {  
            try
            {
                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "scroll", "scrollToBottom();", true);     // Scroll page all the way down

                // Get id of edit icon button then hide it
                string last = string.Empty;
                LinkButton b = new LinkButton();

                if (sender is LinkButton)
                {
                    b = (LinkButton)sender;
                    if (b.ID.Contains("edit"))
                    {
                        // Make sure current edit is done before starting another one
                        if (labelControlsTable.Visible == true)
                        {
                            string msg = "Finish or close current Control Setup.";
                            onScreenMsg(msg, "#ff3333;", "configSection");
                            return;
                        }
                        ViewState["senderID"] = b.ID;
                        last = b.ID.Substring(b.ID.Length - 1, 1);
                        if (last.Equals("0")) last = "10";
                    }
                }
                else
                {
                    for (int i = 1; i <= 10; i++)
                    {
                        LinkButton btn = this.Master.FindControl("MainContent").FindControl("edit" + i) as LinkButton;
                        if (btn != null)
                        {
                            if (btn.Visible == false)
                                last = i.ToString();
                        }
                        else last = "1";
                    }
                }
                
                b.Visible = false;

                // Make sure the 1st control is set 
                if (last != "1" && label1.Text == " Required")
                {
                    string msg = "LABEL1 must be set first.";
                    onScreenMsg(msg, "#ff3333;", "configSection");
                    b.Visible = true;
                    return;
                }


                // Get selected job id
                int jobId = Helper.getJobId(selectJob.SelectedValue);
                if (jobId <= 0)
                {
                    string msg = "Job selected could not be found. Contact system admin.";
                    ScriptManager.RegisterStartupScript(Page,Page.GetType(), "myalert", "alert('" + msg + "');", true);
                    selectJob.SelectedValue = "Select";
                    return;
                }

                // Clear labelControlsTable current values
                labelTextBox.Text = string.Empty;
                dropdownValues.Text = string.Empty;
                regexTextBox.Text = string.Empty;
                msgTextBox.Text = string.Empty;

                // Retrieve DB saved values
                var controlInfo = isDropdownType(Convert.ToInt32(last), jobId);
                bool isLabel1Set = controlInfo.Item1;
                bool isControlDropdown = controlInfo.Item2;

                // Check control type
                if (isControlDropdown == true)
                {
                    dropdownType.Checked = true;
                    textBoxType.Checked = false;
                    dropdownValues.Visible = true;
                    valuesLabel.Visible = true;
                    regexTextBox.Visible = false;
                    msgTextBox.Visible = false;
                    regex.Visible = false;
                    alert.Visible = false;
                    TextBox t = this.Master.FindControl("MainContent").FindControl("label" + last) as TextBox;
                    if (t.Text == string.Empty)
                    {
                        labelTextBox.Text = string.Empty;
                        string placeholder = string.Empty;
                        if (last == "1") placeholder = " Required";
                        else placeholder = " Optional";
                        labelTextBox.Attributes["placeholder"] = placeholder;
                    }
                    else labelTextBox.Text = t.Text.Trim();
                    string values = getDropdownValues(jobId, Convert.ToInt32(last));
                    dropdownValues.Text = " " + values.Trim();
                }
                else
                {
                    textBoxType.Checked = true;
                    dropdownType.Checked = false;
                    dropdownValues.Visible = false;
                    valuesLabel.Visible = false;
                    regexTextBox.Visible = true;
                    msgTextBox.Visible = true;
                    regex.Visible = true;
                    alert.Visible = true;

                    if (isLabel1Set == true)  // If textbox type
                    {
                       int success = showControlInfo(jobId, Convert.ToInt32(last));
                       if (success == -1)
                       {
                            string msg = "Error 74a: Issue occured while retrieving saved index data. Contact system admin.";
                            ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                       }
                    }
                    else
                    {
                        string placeholder = string.Empty;
                        if (last == "1") placeholder = "Required";
                        else placeholder = "Optional";
                        labelTextBox.Attributes["placeholder"] = placeholder;
                        regexTextBox.Attributes["placeholder"] = "Optional.";
                        msgTextBox.Attributes["placeholder"] = "Message of what a valid entry should be.";
                    }
                }
                labelControlsTable.Visible = true;
                setupTitle.Visible = true;
                labelTextBox.Focus();
            }
            catch(Exception ex)
            {
                string msg = "Error 74b: Issue occured while retrieveing saved index data. Contact sytem admin.";
                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + ex.Message + "');", true);
                // Log the exception and notify system operators
                ExceptionUtility.LogException(ex);
                ExceptionUtility.NotifySystemOps(ex);
            } 
        }


        // 'TYPE' RADIO GROUP CHECKED.
        protected void radioBtnChanged_Click(object sender, EventArgs e)
        {
            ScriptManager.RegisterStartupScript(Page, Page.GetType(), "scroll", "scrollToBottom();", true);     // Scroll page all the way down
            if (dropdownType.Checked)
            {
                dropdownValues.Visible = true;
                valuesLabel.Visible = true;
                labelTextBox.Focus();
                regexTextBox.Visible = false;
                msgTextBox.Visible = false;
                regex.Visible = false;
                alert.Visible = false;
            }
            else
            {
                dropdownValues.Visible = false;
                valuesLabel.Visible = false;
                labelTextBox.Focus();
                regexTextBox.Visible = true;
                msgTextBox.Visible = true;
                regex.Visible = true;
                alert.Visible = true;
            }
        }


        // PREVENT LINE BREAKS IN GRIDVIEW
        protected void rowDataBound(object sender, GridViewRowEventArgs e)
        {
            try
            {
                Control c = new Control();
                c = (Control)sender;
                string id = c.ID;
                //if (b.ID.Contains("edit"))

                // GIVE CUSTOM COLUMN NAMES
                if (e.Row.RowType == DataControlRowType.Header)
                {
                    e.Row.Cells[2].Text = "JOB";
                    if (e.Row.Cells.Count == 5)
                    {
                        e.Row.Cells[0].Visible = false; // Hide 1st col header if only listing active jobs
                        e.Row.Cells[3].Text = "INDEX";
                        e.Row.Cells[4].Text = "PRINTED";
                        //e.Row.Cells[5].Text = "UNPRNT";
                    } 

                    string colBorder = "border-left:1px solid #737373; border-right:1px solid #737373; white-space: nowrap;";
                    for (int i = 0; i < e.Row.Cells.Count; i++)
                        e.Row.Cells[i].Attributes.Add("style", colBorder);
                }
                // Set column borders & Prevent line breaks
                if (e.Row.RowType == DataControlRowType.DataRow)
                {
                    string colBorder = "border-width:1px 1px 1px 1px; border-style:solid; border-color:#cccccc; white-space: nowrap;";
                    for (int i = 0; i < e.Row.Cells.Count; i++)
                        e.Row.Cells[i].Attributes.Add("style", colBorder);

                    if (e.Row.Cells.Count == 5) e.Row.Cells[0].Visible = false; // Hide 1st col cell if only listing active jobs
                }

                if (e.Row.RowType == DataControlRowType.Pager)
                {
                    string colBorder = "border-left:1px solid #646464; border-right:1px solid #646464; white-space: nowrap;";
                    for (int i = 0; i < e.Row.Cells.Count; i++)
                        e.Row.Cells[i].Attributes.Add("style", colBorder);

                    if (e.Row.Cells.Count == 5) e.Row.Cells[0].Visible = false; // Hide 1st col pager if only listing active jobs
                }
            }
            catch (Exception ex)
            {
                string msg = "Error 47: Issue occured while attempting to prevent line breaks within gridview. Contact system admin.";
                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                // Log the exception and notify system operators
                ExceptionUtility.LogException(ex);
                ExceptionUtility.NotifySystemOps(ex);
            }
        }


        // COLLAPSE ALL SECTIONS. 
        protected void collapseAll_Click(object sender, EventArgs e)
        {   
            try
            {
                if (downArrow.Visible == true)
                {
                    downArrow.Visible = false;
                    upArrow.Visible = true;
                    jobSection.Visible = true;
                    newUserSection.Visible = true;
                    getUnassignedJobs(null);
                    getOperators();
                    assignPanel.Visible = true;
                    jobIndexEditingPanel.Visible = true;
                    getActiveJobs();
                    jobSectionDefault();
                    line.Visible = true;
                    labelsTable.Visible = false;
                    labelControlsTable.Visible = false;
                    setupTitle.Visible = false;
                    //labelDropdown_Click(new object(), new EventArgs());
                }
                else
                {
                    downArrow.Visible = true;
                    upArrow.Visible = false;
                    jobSection.Visible = false;
                    newUserSection.Visible = false;
                    assignPanel.Visible = false;
                    jobIndexEditingPanel.Visible = false;
                    line.Visible = false;
                }
            }
            catch (Exception ex)
            {
                string msg = "Error 79: Issue occured while attempting to hile or collapse all sections. Contacy system admin.";
                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                // Log the exception and notify system operators
                ExceptionUtility.LogException(ex);
                ExceptionUtility.NotifySystemOps(ex);
            }
        }



        // GET ALL ACTIVE JOBS. HELPER FUNCTION
        private void getUnassignedJobs(Object sender)
        {
            try
            {
                string assigneeName = string.Empty;
                int opID = 0;
                SqlCommand cmd = null;
                Button button = (Button)sender;
                string buttonId = string.Empty;
                if (button != null) buttonId = button.ID;

                using (SqlConnection con = Helper.ConnectionObj)
                {
                    using (cmd = con.CreateCommand())
                    {
                        // If 'INACCESSIBLE' clicked
                        if (button != null && (buttonId == "inaccessibleBtn" || buttonId == "jobAccessBtn"))
                        {
                            ScriptManager.RegisterStartupScript(Page, Page.GetType(), "scroll", "scrollToBottom();", true);     // Scroll page all the way down

                            // Make sure Operator is entered
                            if (assignee.SelectedValue != "Select") assigneeName = assignee.SelectedValue;
                            else
                            {
                                onScreenMsg("Required", "#ff3333;", "jobAccessSection1");
                                return;
                            }
                            
                            // Then check if specified operator exists. If so, get ID.
                            cmd.CommandText = "SELECT ID FROM OPERATOR WHERE NAME = @assignedTo";
                            cmd.Parameters.AddWithValue("@assignedTo", assigneeName);
                            con.Open();
                            object result = cmd.ExecuteScalar();
                            if (result != null) opID = (int)result;
                            else
                            {
                                string msg = "Operator entered could not be found.";
                                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                                return;
                            }

                            // Then, set cmd to retrieve operator's inaccessible jobs
                            cmd.Parameters.Clear();
                            cmd.CommandText = "SELECT ABBREVIATION " +
                                              "FROM JOB " +
                                              "WHERE ACTIVE=1 AND ID NOT IN (SELECT JOB_ID FROM OPERATOR_ACCESS WHERE OPERATOR_ID=@opId) " +
                                              "ORDER BY ABBREVIATION ASC";
                            cmd.Parameters.AddWithValue("@opId", opID);
                        }
                        else
                        {
                            // If 'INACCESSIBLE' not clicked, set cmd to retrieve all Active jobs.
                            cmd.Parameters.Clear();
                            cmd.CommandText = "SELECT ABBREVIATION, COUNT(INDEX_DATA.ID), COUNT(CASE WHEN INDEX_DATA.PRINTED=1 THEN 1 END) " +
                                               "FROM JOB " +
                                               "LEFT JOIN INDEX_DATA ON JOB.ID=INDEX_DATA.JOB_ID " +
                                               "WHERE ACTIVE=1 " +
                                               "GROUP BY ABBREVIATION " +
                                               "ORDER BY ABBREVIATION ASC";
                                                // "LEFT JOIN OPERATOR_ACCESS ON JOB.ID = OPERATOR_ACCESS.JOB_ID " +   //In case we want jobs inaccessible by everyone
                                                // "WHERE JOB.ACTIVE = 1 AND OPERATOR_ACCESS.JOB_ID IS NULL", con);"
                        }

                        using (SqlDataAdapter da = new SqlDataAdapter(cmd))
                        {
                            DataSet ds = new DataSet();
                            da.Fill(ds);
                            if (ds.Tables.Count > 0)
                            {
                                jobAccessGridView.DataSource = ds.Tables[0];
                                jobAccessGridView.DataBind();
                                jobAccessGridView.Visible = true;
                            }

                            // Handling of whether any JOB was returned from DB
                            if (jobAccessGridView.Rows.Count == 0)
                            {
                                jobAccessGridView.Visible = false;
                                jobAccessBtn.Visible = false;
                                deleteAssignedBtn.Visible = false;
                                if (buttonId == "inaccessibleBtn" || buttonId == "jobAccessBtn") jobsLabel.Text = "No Inaccessible Jobs";
                                else jobsLabel.Text = "No Active Jobs Found";
                                jobsLabel.Visible = true;
                            }
                            else
                            {
                                if (buttonId == "inaccessibleBtn" || buttonId == "jobAccessBtn")
                                {
                                    jobsLabel.Text = "Inaccessible Jobs";
                                    jobAccessBtn.Visible = true;
                                }
                                else
                                {
                                    jobsLabel.Text = "Active Jobs";
                                    jobAccessBtn.Visible = false;
                                }
                                
                                jobsLabel.Visible = true;
                                deleteAssignedBtn.Visible = false;
                            }
                        }
                    }
                }
            }
            catch (Exception ex)
            {
                string msg = "Error 81: Issue occured while attempting to retrieve jobs. Contact system admin.";
                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                // Log the exception and notify system operators
                ExceptionUtility.LogException(ex);
                ExceptionUtility.NotifySystemOps(ex);
            }
        }



        // Get dropdown list job items
        private void getDropdownJobItems()
        {
            try 
            {
                selectJobList.Items.Clear();
                selectJobList.Items.Add("Select");

                using (SqlConnection con = Helper.ConnectionObj)
                {
                    using (SqlCommand cmd = con.CreateCommand())
                    {
                        cmd.CommandText = "SELECT ABBREVIATION, ACTIVE FROM JOB ORDER BY ABBREVIATION ASC";
                        con.Open();
                        using (SqlDataReader reader = cmd.ExecuteReader())
                        {
                            if (reader.HasRows)
                            {
                                while (reader.Read())
                                {
                                    string jobAbb = (string)reader.GetValue(0);
                                    bool active = (bool)reader.GetValue(1);
                                    selectJobList.Items.Add(jobAbb);
                                    if (active)
                                    {
                                        // Red config job items from 'JOB' section
                                        foreach (ListItem item in selectJobList.Items)
                                        {
                                            if (item.Value == jobAbb)
                                            {
                                                item.Attributes.Add("style", "color:#009900;font-weight:bold;");
                                            }
                                        }
                                    }
                                }
                            }
                            else
                            {
                                string msg = "No Jobs could be found.";
                                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                                return;
                            }
                        }
                    }
                }
            }
            catch(Exception ex) 
            {
                string msg = "Error 84: Issue occured while attempting to retrieve jobs. Contact system admin. " ;
                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                // Log the exception and notify system operators
                ExceptionUtility.LogException(ex);
                ExceptionUtility.NotifySystemOps(ex);
            }
        }



        // Get dropdown list ACTIVE job items
        private void getActiveJobs()
        {
            try 
            {
                selectJob.Items.Clear();
                selectJob.Items.Add("Select");
                using (SqlConnection con = Helper.ConnectionObj)
                {
                    using (SqlCommand cmd = con.CreateCommand())
                    {   
                        // Get all active jobs
                        cmd.CommandText = "SELECT ABBREVIATION FROM JOB WHERE ACTIVE=1 ORDER BY ABBREVIATION ASC";
                        con.Open();
                        using (SqlDataReader reader = cmd.ExecuteReader())
                        {
                            if (reader.HasRows)
                            {
                                while (reader.Read())
                                {
                                    string jobAbb = (string)reader.GetValue(0);
                                    selectJob.Items.Add(jobAbb);
                                }
                            }
                            else
                            {
                                string msg = "No Active jobs could be found";
                                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                                return;
                            }
                        }
                    }
                }
                setDropdownColor();
            }
            catch(Exception ex) 
            {
                string msg = "Error 86: Issue occured while attempting to retrieve active jobs. Contact system admin. ";
                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                // Log the exception and notify system operators
                ExceptionUtility.LogException(ex);
                ExceptionUtility.NotifySystemOps(ex);
            }
        }



        // ASSIGN JOB TO OPERATOR. HELPER FUNCTION
        private bool AssignJob(string assignee, string abbr)
        {
            try
            {
                int opID = 0, jobID = 0;
                using (SqlConnection con = Helper.ConnectionObj)
                {
                    using (SqlCommand cmd = con.CreateCommand())
                    {
                        // First check if specified operator exists. If so, get operator ID.
                        cmd.CommandText = "SELECT ID FROM OPERATOR WHERE NAME = @assignedTo";
                        cmd.Parameters.AddWithValue("@assignedTo", assignee);
                        con.Open();
                        object result = cmd.ExecuteScalar();
                        if (result != null) opID = (int)result;
                        else
                        {
                            string msg = "Specified operator could not be found!";
                            ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                            return false;
                        }

                        // Then, get ID of job to be assigned.
                        cmd.Parameters.Clear();
                        cmd.CommandText = "SELECT ID FROM JOB WHERE ABBREVIATION = @abbr";
                        cmd.Parameters.AddWithValue("@abbr", abbr);
                        try {
                            object result2 = cmd.ExecuteScalar();
                            if (result != null) jobID = (int)result2;
                        }
                        catch (SqlException ex) {
                            string msg = "Error 88: Issue occured while attempting to retrieve job ID. Contact system admin. ";
                            ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                            // Log the exception and notify system operators
                            ExceptionUtility.LogException(ex);
                            ExceptionUtility.NotifySystemOps(ex);
                        }

                        // Now, Save operator ID & new job ID in OPERATOR_ACCESS
                        if (opID > 0 && jobID > 0)
                        {
                            cmd.Parameters.Clear();
                            cmd.CommandText = "INSERT INTO OPERATOR_ACCESS (OPERATOR_ID, JOB_ID) VALUES(@opId, @jobID)";
                            cmd.Parameters.AddWithValue("@opID", opID);
                            cmd.Parameters.AddWithValue("@jobID", jobID);
                            if (cmd.ExecuteNonQuery() == 1) return true;
                        }
                        return false;
                    }
                }
            }
            catch (Exception ex)
            {
                // Log the exception and notify system operators
                ExceptionUtility.LogException(ex);
                ExceptionUtility.NotifySystemOps(ex);

                // Skip jobs that have already been made accessible to specified operator.
                if (ex.Message.Contains("Violation of UNIQUE KEY") || ex.Message.Contains("Violation of PRIMARY KEY"))
                {
                    return false;
                }
                else
                {
                    string msg = "Error 88: Issue occured while attempting to assign " + abbr + " to operator. Contact system admin. ";
                    ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                }
                return false;
            }
        }



        // SET COLOR FOR DROPDOWN CONFIGURED JOB ITEMS. HELPER FUNCTION
        private void setDropdownColor()
        {   
            try
            {
                using (SqlConnection con = Helper.ConnectionObj)
                {
                    using (SqlCommand cmd = con.CreateCommand())
                    {
                        cmd.CommandText =   "SELECT ABBREVIATION " +
                                            "FROM JOB " +
                                            "INNER JOIN JOB_CONFIG_INDEX ON JOB.ID = JOB_CONFIG_INDEX.JOB_ID " +
                                            "WHERE JOB.ACTIVE = 1";
                        con.Open();
                        using (SqlDataReader reader = cmd.ExecuteReader())
                        {
                            if (reader.HasRows)
                            {
                                while (reader.Read())
                                {
                                    // Red config job items from 'JOB INDEX CONFIG' section
                                    foreach (ListItem item in selectJob.Items)
                                    {
                                        if (item.Value == (string)reader.GetValue(0))
                                        {
                                            item.Attributes.Add("style", "color:#009900;font-weight:bold;");
                                        }
                                    }
                                }
                            }
                        }
                    }
                }
            }
            catch (Exception ex)
            {
                string msg = "Error 90: Issue occured while attempting to color appropriate jobs. Contact system admin. ";
                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                // Log the exception and notify system operators
                ExceptionUtility.LogException(ex);
                ExceptionUtility.NotifySystemOps(ex);
            }
        }




        // SET COLOR FOR DROPDOWN ACTIVE JOB ITEMS. HELPER FUNCTION
        private void jobSectionDropdownColor()
        {
            try
            {
                using (SqlConnection con = Helper.ConnectionObj)
                {
                    using (SqlCommand cmd = con.CreateCommand())
                    {
                        cmd.CommandText = "SELECT ABBREVIATION " +
                                          "FROM JOB " +
                                          "WHERE JOB.ACTIVE = 1";
                        con.Open();
                        using (SqlDataReader reader = cmd.ExecuteReader())
                        {
                            if (reader.HasRows)
                            {
                                while (reader.Read())
                                {
                                    // Red active job items from 'JOB' section
                                    foreach (ListItem item in selectJobList.Items)
                                    {
                                        if (item.Value == (string)reader.GetValue(0))
                                        {
                                            item.Attributes.Add("style", "color:#009900;font-weight:bold;");
                                        }
                                    }
                                }
                            }
                        }
                    }
                }
            }
            catch (Exception ex)
            {
                string msg = "Error 90a: Issue occured while attempting to color appropriate jobs. Contact system admin. ";
                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                // Log the exception and notify system operators
                ExceptionUtility.LogException(ex);
                ExceptionUtility.NotifySystemOps(ex);
            }
        }



        // HANDLE NEXT PAGE CLICK. FUNCTION
        protected void pageChange_Click(object sender, GridViewPageEventArgs e)
        {   
            try
            {
                jobAccessGridView.PageIndex = e.NewPageIndex;
                object b = this.Master.FindControl("MainContent").FindControl("inaccessibleBtn") as Button;

                if (jobsLabel.Text == "Accessible Jobs")
                    getAssignedJobs();
                else if (jobsLabel.Text == "Inaccessible Jobs")
                    getUnassignedJobs(b);
                else
                    getUnassignedJobs(null);
            }
            catch (Exception ex)
            {
                string msg = "Error 91: Issue occured while attempting to operator's ID. Contact system admin. " ;
                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                // Log the exception and notify system operators
                ExceptionUtility.LogException(ex);
                ExceptionUtility.NotifySystemOps(ex);
            }
        }



        // CLEAR RULES. HELPER FUNCTION
        private void clearRules()
        {
            selectJob.SelectedValue = "Select";
            label1.Text = string.Empty;
            label1.Attributes["placeholder"] = "Required";
            label1.Focus();
            label2.Text = string.Empty;
            label2.Attributes["placeholder"] = "Optional";
            label3.Text = string.Empty;
            label3.Attributes["placeholder"] = "Optional";
            label4.Text = string.Empty;
            label4.Attributes["placeholder"] = "Optional";
            label5.Text = string.Empty;
            label5.Attributes["placeholder"] = "Optional";
            for (int i = 1; i <= 10; i++) ViewState["labelValuesedit" + i] = null;
        }


        // JOB SECTION DEFAULTS. HELPER FUNCTION
        private void jobSectionDefault()
        {
            selectAction.SelectedValue = "create";
            selectJobList.Visible = false;
            deleteJobBtn.Visible = false;
            editJobBtn.Visible = false;

            jobAbb.Visible = true;
            createJobBtn.Visible = true;
            jobNameLabel.Visible = true;
            jobName.Visible = true;
            jobName.Attributes["placeholder"] = " Required";
            jobActiveLabel.Visible = true;
            jobActiveBtn.Visible = true;
            jobActiveBtn.SelectedValue = "1";
            jobAssignedToLabel.Visible = true;
            jobAssignedTo.Visible = true;
            getOperators();
        }

        // CHECK CONTROL TYPE
        private Tuple<bool,bool> isDropdownType(int order, int jobId)
        {
            using (SqlConnection con = Helper.ConnectionObj)
            {
                using (SqlCommand cmd = con.CreateCommand())
                {
                    cmd.CommandText = "SELECT LABEL" + order + ", TABLEID" + order + " FROM JOB_CONFIG_INDEX WHERE JOB_ID=@jobID";
                    cmd.Parameters.AddWithValue("@jobID", jobId);
                    con.Open();
                    using (SqlDataReader reader = cmd.ExecuteReader())
                    {
                        if (reader.HasRows && reader.Read())
                        {
                            // If Control type = dropdown, hide textbox & associates
                            if (reader.GetValue(0) != DBNull.Value)
                            {
                                if (reader.GetValue(1) != DBNull.Value)
                                    return Tuple.Create(true,true);
                                return Tuple.Create(true, false);
                            }
                            return Tuple.Create(true, false);
                        }
                        return Tuple.Create(false,false);
                    }
                }
            }
        }


        // GET DROPDOWN VALUES
        private string getDropdownValues(int jobId, int order)
        {
            using (SqlConnection con = Helper.ConnectionObj)
            {
                using (SqlCommand cmd = con.CreateCommand())
                {
                    string values = string.Empty;
                    cmd.CommandText = "SELECT VALUE FROM INDEX_TABLE_FIELD WHERE ID=@id";
                    cmd.Parameters.AddWithValue("@id", jobId + order.ToString());
                    con.Open();
                    using (SqlDataReader reader = cmd.ExecuteReader())
                    {
                        if (reader.HasRows)
                        {
                            while (reader.Read())
                            {
                                values += reader.GetValue(0).ToString() + ", ";
                            }
                            values = values.Substring(0, values.Length - 2);
                            return values;
                        }
                        return string.Empty;
                    }
                }
            }
        }

        // SAVE DROPDOWN VALUES
        private int saveDropdownValues (int jobId, string value, int controlBeingSet, int dropdownOrder)
        {   
            try
            {
                using (SqlConnection con = Helper.ConnectionObj)
                {
                    using (SqlCommand cmd = con.CreateCommand())
                    {
                        // Save new table id
                        cmd.CommandText = "INSERT INTO INDEX_TABLE_FIELD(ID, VALUE, ORD) VALUES(@id, @value, @ord)";
                        cmd.Parameters.AddWithValue("@id", jobId + controlBeingSet.ToString());
                        cmd.Parameters.AddWithValue("@value", value);
                        cmd.Parameters.AddWithValue("@ord", dropdownOrder);
                        con.Open();
                        if (cmd.ExecuteNonQuery() == 1)
                        {
                            cmd.Parameters.Clear();
                            return 1;
                        }
                        return 0;
                    }
                }
            }
            catch(Exception ex)
            {
                // Log the exception and notify system operators
                ExceptionUtility.LogException(ex);
                ExceptionUtility.NotifySystemOps(ex);
                return -1; // Error
            }
        }

        // DELETE DROPDOWN VALUES
        private int deleteDropdownValues(int jobId, int order)
        {
            try
            {
                using (SqlConnection con = Helper.ConnectionObj)
                {
                    using (SqlCommand cmd = con.CreateCommand())
                    {
                        // Clear table id if exists
                        con.Open();
                        cmd.CommandText = "DELETE FROM INDEX_TABLE_FIELD WHERE ID=@id";
                        cmd.Parameters.AddWithValue("@id", jobId + order.ToString());
                        return cmd.ExecuteNonQuery();
                    }
                }
            }
            catch (Exception ex)
            {
                // Log the exception and notify system operators
                ExceptionUtility.LogException(ex);
                ExceptionUtility.NotifySystemOps(ex);
                return -1; // Error
            }
        }


        // SAVE JOB CONFIG
        private int saveJobConfig(int jobId, int order, string controlType, string sqlType)
        {
            try
            {
                using (SqlConnection con = Helper.ConnectionObj)
                {
                    using (SqlCommand cmd = con.CreateCommand())
                    {
                        // Now set job config index
                        con.Open();

                        // If 1st control aka Job not yet configured
                        if (sqlType == "insert") 
                        {
                            cmd.CommandText =   "INSERT INTO JOB_CONFIG_INDEX" +
                                                "(JOB_ID, LABEL1, REGEX1, ALERT1, TABLEID1, LABEL2, REGEX2, ALERT2, TABLEID2, LABEL3, REGEX3, ALERT3, TABLEID3, " +
                                                "LABEL4, REGEX4, ALERT4, TABLEID4, LABEL5, REGEX5, ALERT5, TABLEID5, LABEL6, REGEX6, ALERT6, TABLEID6, " +
                                                "LABEL7, REGEX7, ALERT7, TABLEID7, LABEL8, REGEX8, ALERT8, TABLEID8, LABEL9, REGEX9, ALERT9, TABLEID9, " +
                                                "LABEL10, REGEX10, ALERT10, TABLEID10) " +
                                                "VALUES(@jobID, @label1, @regex1, @alert1, @tableid1, @label2, @regex2, @alert2, @tableid2, " +
                                                "@label3, @regex3, @alert3, @tableid3, @label4, @regex4, @alert4, @tableid4, @label5, @regex5, @alert5, @tableid5, "+
                                                "@label6, @regex6, @alert6, @tableid6, @label7, @regex7, @alert7, @tableid7, @label8, @regex8, @alert8, @tableid8, "+
                                                "@label9, @regex9, @alert9, @tableid9, @label10, @regex10, @alert10, @tableid10)";
                            if (controlType == "dropdown")
                            {
                                for (int i = 1; i <= 10; i++)
                                {
                                    if (i == 1)
                                    {
                                        cmd.Parameters.AddWithValue("@label" + i, labelTextBox.Text.Trim());
                                        if (dropdownValues.Text == string.Empty)
                                            cmd.Parameters.AddWithValue("@tableid" + i, DBNull.Value);
                                        else cmd.Parameters.AddWithValue("@tableid" + i, jobId + order.ToString());
                                    }
                                    else
                                    {
                                        cmd.Parameters.AddWithValue("@label" + i, DBNull.Value);
                                        cmd.Parameters.AddWithValue("@tableid" + i, DBNull.Value);
                                    }
                                    cmd.Parameters.AddWithValue("@regex" + i, DBNull.Value);
                                    cmd.Parameters.AddWithValue("@alert" + i, DBNull.Value);
                                }
                                cmd.Parameters.AddWithValue("@jobID", jobId);
                            }
                            else // If control = textbox
                            {
                                for (int i = 1; i <= 10; i++)
                                {
                                    if (i == 1)
                                    {
                                        cmd.Parameters.AddWithValue("@label" + i, labelTextBox.Text.Trim());
                                        if (regexTextBox.Text != string.Empty)
                                            cmd.Parameters.AddWithValue("@regex" + i, regexTextBox.Text.Trim());
                                        else cmd.Parameters.AddWithValue("@regex" + i, DBNull.Value);
                                        if (msgTextBox.Text != string.Empty)
                                            cmd.Parameters.AddWithValue("@alert" + i, msgTextBox.Text.Trim());
                                        else cmd.Parameters.AddWithValue("@alert" + i, DBNull.Value);
                                    }
                                    else
                                    {
                                        cmd.Parameters.AddWithValue("@label" + i, DBNull.Value);
                                        cmd.Parameters.AddWithValue("@regex" + i, DBNull.Value);
                                        cmd.Parameters.AddWithValue("@alert" + i, DBNull.Value);
                                    }
                                    cmd.Parameters.AddWithValue("@tableid" + i, DBNull.Value);
                                }
                                cmd.Parameters.AddWithValue("@jobID", jobId);
                            }
                            return cmd.ExecuteNonQuery();
                        }
                        else // if job already configured
                        {
                            cmd.CommandText =   "UPDATE JOB_CONFIG_INDEX " +
                                                "SET LABEL" + order + "=@label, REGEX" + order + "=@regex, ALERT" + order + "=@alert, TABLEID" + order + "=@tableid " +
                                                "WHERE JOB_ID=@jobID";

                            // If control type is dropdown
                            if (controlType == "dropdown")
                            {
                                if (labelTextBox.Text == string.Empty)
                                {
                                    cmd.Parameters.AddWithValue("@label", DBNull.Value);
                                    cmd.Parameters.AddWithValue("@tableid", DBNull.Value);
                                }
                                else
                                {
                                    cmd.Parameters.AddWithValue("@label", labelTextBox.Text.Trim());
                                    if (dropdownValues.Text == string.Empty)
                                        cmd.Parameters.AddWithValue("@tableid", DBNull.Value);
                                    else cmd.Parameters.AddWithValue("@tableid", jobId + order.ToString());
                                } 
                                cmd.Parameters.AddWithValue("@regex", DBNull.Value);
                                cmd.Parameters.AddWithValue("@alert", DBNull.Value);
                                cmd.Parameters.AddWithValue("@jobID", jobId);
                            }
                            else    // if control type is textbox
                            {
                                if (labelTextBox.Text == string.Empty)
                                {
                                    cmd.Parameters.AddWithValue("@label", DBNull.Value);
                                    cmd.Parameters.AddWithValue("@regex", DBNull.Value);
                                    cmd.Parameters.AddWithValue("@alert", DBNull.Value);
                                }
                                else
                                {
                                    cmd.Parameters.AddWithValue("@label", labelTextBox.Text.Trim());
                                    if (regexTextBox.Text != string.Empty)
                                        cmd.Parameters.AddWithValue("@regex", regexTextBox.Text.Trim());
                                    else cmd.Parameters.AddWithValue("@regex", DBNull.Value);
                                    if (msgTextBox.Text != string.Empty)
                                        cmd.Parameters.AddWithValue("@alert", msgTextBox.Text.Trim());
                                    else cmd.Parameters.AddWithValue("@alert", DBNull.Value);
                                }
                                cmd.Parameters.AddWithValue("@tableid", DBNull.Value);
                                cmd.Parameters.AddWithValue("@jobID", jobId);
                            }
                            return cmd.ExecuteNonQuery();
                        }
                    }
                }
            }
            catch (Exception ex)
            {
                // Log the exception and notify system operators
                ExceptionUtility.LogException(ex);
                ExceptionUtility.NotifySystemOps(ex);
                return -1; // Error
            }
        }

        // HIDE LABEL INPUT FORM
        private void hideLabelControlsTable()
        {
            setDropdownColor();
            labelControlsTable.Visible = false;
            setupTitle.Visible = false;
            for (int i = 1; i <= 10; i++)
            {
                LinkButton btn = this.Master.FindControl("MainContent").FindControl("edit" + i) as LinkButton;
                TextBox t = this.Master.FindControl("MainContent").FindControl("label" + i) as TextBox;
                if (btn.Visible == false)
                {
                    btn.Visible = true;
                    if (labelTextBox.Text != string.Empty)
                        t.Text = " " + labelTextBox.Text.Trim();
                    else if (labelTextBox.Text == string.Empty && i != 1)
                        t.Text = " Optional";
                }
            }
        }

        // HANDLE RESULT
        private void handleResult(int result, string e)
        {
            if (result != 1)
            {
                string msg = "Error 73" + e + ". Could not save job config. Contact system admin.";
                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                selectJob.SelectedValue = "Select";
                return;
            }
            else
            {
                hideLabelControlsTable();
                return;
            }
        }


        // RETRIEVE TEXTBOX CONTROL TYPE INFOS
        private int showControlInfo (int jobId, int controlBeingSet)
        {
            try
            {
                // Retrieve DB saved values
                using (SqlConnection con = Helper.ConnectionObj)
                {
                    using (SqlCommand cmd = con.CreateCommand())
                    {
                        cmd.CommandText =   "SELECT LABEL" + controlBeingSet + ", REGEX" + controlBeingSet + ", ALERT" + controlBeingSet + " " +
                                            "FROM JOB_CONFIG_INDEX " +
                                            "WHERE JOB_ID=@jobID";
                        cmd.Parameters.AddWithValue("jobID", jobId);
                        con.Open();
                        using (SqlDataReader reader = cmd.ExecuteReader())
                        {
                            if (reader.HasRows)
                            {
                                if (reader.Read() && reader.GetValue(0) != null)
                                {
                                    if (reader.GetValue(0).ToString() != DBNull.Value.ToString())
                                    {
                                        labelTextBox.Text = (string)reader.GetValue(0);
                                        if (reader.GetValue(1).ToString() != string.Empty)
                                            regexTextBox.Text = reader.GetValue(1).ToString();
                                        else
                                            regexTextBox.Attributes["placeholder"] = "Optional. Remove delimiters //";
                                        if (reader.GetValue(2).ToString() != string.Empty)
                                            msgTextBox.Text = reader.GetValue(2).ToString();
                                        else
                                            msgTextBox.Attributes["placeholder"] = "Message of what a valid entry should be.";
                                    }
                                }
                            }
                        }
                    }
                }
                return 1;
            }
            catch(Exception ex)
            {
                // Log the exception and notify system operators
                ExceptionUtility.LogException(ex);
                ExceptionUtility.NotifySystemOps(ex);
                return -1;
            }
        }


        // PRINT VARIOUS MSG ON SCREEN INSTEAD OF A POPUP
        private void onScreenMsg(string msg, string color, string from)
        {
            var screenMsg = new TableCell();
            var screenMsgRow = new TableRow();
            screenMsg.Text = msg;
            screenMsg.Attributes["style"] = "color:" + color;
            screenMsgRow.Cells.Add(screenMsg);
            if (from == "configSection") configMsg.Rows.Add(screenMsgRow);
            if (from == "jobAccessSection1") jobAccessMsg1.Rows.Add(screenMsgRow);
            if (from == "jobAccessSection2") jobAccessMsg2.Rows.Add(screenMsgRow);
            if (from == "userSection") UserSectionMsg.Rows.Add(screenMsgRow);
            if (from == "jobSetupSection") jobSetupMsg.Rows.Add(screenMsgRow);
        }

    }
}
-----------Helper.cs
using System;
using System.Configuration;
using System.Data.SqlClient;
using System.Drawing;
using System.Web.UI;

namespace BarcodeConversion.App_Code
{
    public class Helper
    {
        // GET CONNECTION OBJECT. FUNCTION
        public static SqlConnection ConnectionObj
        {
            get
            {  
                ConnectionStringSettings conString = ConfigurationManager.ConnectionStrings["myConnection"];
                string connectionString = conString.ConnectionString;
                SqlConnection con = new SqlConnection(connectionString);
                return con;
              
            }
        }



        // GET USER ID VIA USERNAME. HELPER FUNCTION
        public static int getUserId(string user)
        {   
            try 
            {
                int opID = 0;
                using (SqlConnection con = ConnectionObj)
                {
                    using (SqlCommand cmd = con.CreateCommand())
                    {
                        cmd.CommandText = "SELECT ID FROM OPERATOR WHERE NAME = @username";
                        cmd.Parameters.AddWithValue("@username", user);
                        con.Open();
                        object result = cmd.ExecuteScalar();
                        if (result != null)
                        {
                            opID = (int)result;
                            return opID;
                        }
                        else return opID;
                    }
                }
            }
            catch (Exception ex)
            {
                throw new Exception(ex.Message);
            }
        }


        // GET JOB ID via job abbreviation
        public static int getJobId(string jobAbb)
        {
            try
            {
                int jobID = 0;
                using (SqlConnection con = Helper.ConnectionObj)
                {
                    using (SqlCommand cmd = con.CreateCommand())
                    {
                        cmd.CommandText = "SELECT ID FROM JOB WHERE ABBREVIATION = @abb";
                        cmd.Parameters.AddWithValue("@abb", jobAbb);
                        con.Open();
                        var result = cmd.ExecuteScalar();
                        if (result != null)
                        {
                            jobID = (int)result;
                            return jobID;
                        }
                        else return jobID;
                    }
                }
            }
            catch (Exception ex)
            {
                return -1; // Error
            }
        }


        // GET CONTROL THAT FIRED POSTBACK. HELPER FUNCTION.
        public static Control GetPostBackControl(Page page)
        {
            try
            {
                Control control = null;

                string ctrlname = page.Request.Params.Get("__EVENTTARGET");
                if (ctrlname != null && ctrlname != string.Empty)
                {
                    control = page.FindControl(ctrlname);
                }
                else
                {
                    foreach (string ctl in page.Request.Form)
                    {
                        Control c = page.FindControl(ctl);
                        if (c is System.Web.UI.WebControls.Button)
                        {
                            control = c;
                            break;
                        }
                    }
                }
                return control;
            }
            catch(Exception ex)
            {
                ExceptionUtility.LogException(ex);
                ExceptionUtility.NotifySystemOps(ex);
                return null;
            }
            
        }
        
    }
}
----------IndexStatus.aspx
<%@ Page Title="Index Status" Language="C#"  MasterPageFile="~/Site.Master" AutoEventWireup="true" CodeBehind="IndexStatus.aspx.cs" Inherits="BarcodeConversion.IndexStatus" %>


<asp:Content ID="BodyContent" ContentPlaceHolderID="MainContent" runat="server">
	
<script src="Scripts/jquery.dynDateTime.min.js" type="text/javascript"></script>
<script src="Scripts/calendar-en.min.js" type="text/javascript"></script>
<link href="Content/calendar-blue.css" rel="stylesheet" type="text/css" />
<script type="text/javascript">
	$(document).ready(function () {
		$("#<%=from.ClientID %>").dynDateTime({
			showsTime: false,
			ifFormat: " %m/%d/%Y",// %H:%M",
			daFormat: "%l;%M %p, %e %m, %Y",
			align: "BR",
			electric: false,
			singleClick: false,
			displayArea: ".siblings('.dtcDisplayArea')",
			button: ".next()"
		});
		$("#<%=to.ClientID %>").dynDateTime({
			showsTime: false,
			ifFormat: " %m/%d/%Y",// %H:%M",
			daFormat: "%l;%M %p, %e %m, %Y",
			align: "BR",
			electric: false,
			singleClick: false,
			displayArea: ".siblings('.dtcDisplayArea')",
			button: ".next()"
		});
	});
</script>

	 <asp:Panel ID="indexStatusPanel" runat="server">
		<div style="margin-top:45px; height:50px; border-bottom:solid 1px green;width:899px;">
			<table style="width:899px;">
				<tr>
					<td><h3 style="display:inline; padding-top:25px;color:#595959">Index Status Report</h3></td>
				</tr>
			</table>
		</div>  
		<div style="display:inline-block;">
            <asp:UpdatePanel runat="server">
                <ContentTemplate>
			        <asp:Panel CssClass="card" style="background-color:#e6f3ff;margin-top:20px;" runat="server">
				        <table class = "table" style="width:99%;">
					        <tr >
						        <td><asp:Label ID="filterLabel" Width="55" runat="server"><h5>Filter :</h5></asp:Label></td>
						        <td style="padding-top:14px;"> 
							        <asp:DropDownList ID="jobsFilter" OnSelectedIndexChanged="onSelectedChange" onmousedown="this.focus()" runat="server">
								        <asp:ListItem Value="allJobs">Your Jobs</asp:ListItem>
							        </asp:DropDownList>
						        </td>
						        <td style="padding-top:14px;"> 
							        <asp:DropDownList ID="whoFilter" OnSelectedIndexChanged="onSelectedChange" onmousedown="this.focus()" runat="server" AutoPostBack="true">
								        <asp:ListItem Value="meOnly">Your Indexes Only</asp:ListItem>
								        <asp:ListItem Value="everyone">Indexes for all Operators</asp:ListItem>
							        </asp:DropDownList>
						        </td>
						        <td style="padding-top:14px;"> 
							        <asp:DropDownList ID="whenFilter" OnSelectedIndexChanged="onSelectWhen" onmousedown="this.focus()" runat="server" AutoPostBack="true">
								        <asp:ListItem Value="allTime">For All Time</asp:ListItem>
								        <asp:ListItem Value="pickRange">Pick Date Range</asp:ListItem>
							        </asp:DropDownList>
						        </td>
						        <td style="padding-top:14px;"> 
							        <asp:DropDownList ID="whatFilter" OnSelectedIndexChanged="onSelectedChange" onmousedown="this.focus()" runat="server" AutoPostBack="true">
								        <asp:ListItem Value="allSheets">All Sheets</asp:ListItem>
								        <asp:ListItem Value="printed">Printed Only</asp:ListItem>
								        <asp:ListItem Value="notPrinted">Not Printed</asp:ListItem>
							        </asp:DropDownList>
						        </td>
						        <td style="text-align:right;"> 
							        <div style="display:block;padding:10px 5px 1px 4px;" >
								        <asp:linkButton runat="server" ForeColor="#737373" OnClick="reset_Click" >
									        <i class="fa fa-refresh" Visible="true" Width="26" Height="26" BackColor="#e6f3ff" runat="server" ></i>
								        </asp:linkButton>
							        </div>
						        </td>
					        </tr>
				        </table> 
			        </asp:Panel>
                </ContentTemplate>
            </asp:UpdatePanel>          
			
			<asp:Panel ID="timePanel" Visible="false" runat="server">
				<table class = "table" style="width:auto;margin-top:15px;margin-left:17px;">
					<tr>
						<td><asp:label runat="server">From:&nbsp;&nbsp;</asp:label>
							<asp:TextBox ID="from" style="display:inline;" ReadOnly="True" runat="server"></asp:TextBox>
						    <asp:Image ImageUrl="Content/calender.png" Visible="true" runat="server"/></td>
						<td style="padding-left:25px;"><asp:label runat="server">To:&nbsp;&nbsp;</asp:label>
							<asp:TextBox ID="to" style="display:inline;" ReadOnly="True" runat="server"></asp:TextBox>
						    <asp:Image ImageUrl="Content/calender.png" Visible="true" runat="server"/></td>
						<td>
							<asp:Button ID="dates" CssClass="btn btn-primary" style="padding:1px 10px 0px 10px;margin:1px 0px 0px 25px;" Font-Size="9" Text="Submit" runat="server" onclick="submit_Click" />
						</td>
					</tr>
				</table>
			</asp:Panel>

            <asp:UpdatePanel runat="server">
                <Triggers>
                    <asp:PostBackTrigger ControlID="whenFilter" />
                </Triggers>
                <ContentTemplate>
                    <asp:Table id="dateAlertMsg" style="margin-left:22px;" runat="server"></asp:Table>
			        <h5 style="color:#4d4dff;margin-top:30px;"><asp:Label ID="description" Visible="false" runat="server"></asp:Label></h5>
                    <asp:Panel ID="gridContainer" style="display:inline-block;" runat="server"> <%--CssClass="card"--%>
				        <table id="gridHeader" style="width:100%;margin-top:15px;margin-bottom:-10px;font-size:12px;" runat="server">
					        <tr>
						        <td><asp:Label ID="sortOrder" Text="Sorted By : CREATION_TIME ASC" runat="server"></asp:Label><asp:Label id="sortDirection" Font-Size="8" runat="server"></asp:Label></td>
						        <td style="text-align:right;padding-right:50px;">
							        <asp:Label ID="recordsPerPageLabel" Text="Records per page " style="padding-right:5px;" runat="server"></asp:Label>
							        <asp:DropDownList ID="recordsPerPage" OnSelectedIndexChanged="onSelectedRecordsPerPage" style="padding:0px 10px 0px 5px;" onmousedown="this.focus()" runat="server" AutoPostBack="true">
								        <asp:ListItem Value="10" Selected="true">10</asp:ListItem>
								        <asp:ListItem Value="15">15</asp:ListItem>
								        <asp:ListItem Value="20">20</asp:ListItem>
								        <asp:ListItem Value="30">30</asp:ListItem>
								        <asp:ListItem Value="50">50</asp:ListItem>
								        <asp:ListItem Value="all">ALL</asp:ListItem>
							        </asp:DropDownList>
						        </td>
					        </tr>
				        </table>
			
				        <asp:GridView ID="indexeStatusGridView" runat="server" style="margin-top:15px" CssClass="mydatagrid" PagerStyle-CssClass="pager"
							        HeaderStyle-CssClass="header" RowStyle-CssClass="rows" AllowPaging="true" OnPageIndexChanging="pageChange_Click"
							        OnRowDataBound="rowDataBound" OnSorting="gridView_Sorting" AllowSorting="True">  
					        <columns>
						        <asp:templatefield HeaderText ="&nbsp;N°" ShowHeader="true">
							        <ItemTemplate >
								        <%# Container.DataItemIndex + 1 %>
							        </ItemTemplate>
						        </asp:templatefield>
					        </columns>       
				        </asp:GridView>
                    </asp:Panel>
                </ContentTemplate>
			</asp:UpdatePanel>
		
		</div>     
	</asp:Panel>
</asp:Content>
------------IndexStatus.aspx.cs
using BarcodeConversion.App_Code;
using System;
using System.Collections.Generic;
using System.Data;
using System.Data.SqlClient;
using System.Linq;
using System.Web;
using System.Web.UI;
using System.Web.UI.WebControls;

namespace BarcodeConversion
{
    public partial class IndexStatus : System.Web.UI.Page
    {
        public void Page_Init(object o, EventArgs e)
        {
            Page.MaintainScrollPositionOnPostBack = true;
        }

        protected void Page_Load(object sender, EventArgs e)
        {
            try
            {
                if (!IsPostBack) // First page load
                {
                    Session["jobsList"] = populateJobsList();
                    getIndexes("Your Jobs", "meOnly", "allTime", "allSheets");
                    indexeStatusGridView.Visible = true;
                }

                // Reset gridview page
                Control c = Helper.GetPostBackControl(this.Page);
                if (c != null && (c.ID == "resetBtn" || c.ID == "whoFilter" || c.ID == "whenFilter" ||
                    c.ID == "whatFilter" || c.ID == "recordsPerPage")) indexeStatusGridView.PageIndex = 0;
                
                // Persits date entries
                if (c != null && (c.ID == "jobsFilter" || c.ID == "whoFilter" ||
                    c.ID == "whatFilter" || c.ID == "dates"))
                {
                    if (timePanel.Visible == true) 
                    {
                        if (ViewState["from"] != null) from.Text = ViewState["from"].ToString();
                        if (ViewState["to"] != null) to.Text = ViewState["to"].ToString();
                    }
                }
                    
            }
            catch (Exception ex)
            {
                string msg = "Error 37: Issue occured while loading this page. Contact your system admin.";
                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                // Log the exception and notify system operators
                ExceptionUtility.LogException(ex);
                ExceptionUtility.NotifySystemOps(ex);
            }
        }



        // 'RESET' CLICKED: RESET FILTER TO DEFAULT VALUES. FUNCTION
        protected void reset_Click(object sender, EventArgs e)
        {
            try
            {
                jobsFilter.SelectedValue = "Your Jobs";
                whoFilter.SelectedValue = "meOnly";
                whenFilter.SelectedValue = "allTime";
                whatFilter.SelectedValue = "allSheets";
                getIndexes("Your Jobs", "meOnly", "allTime", "allSheets");
                indexeStatusGridView.Visible = true;
                indexeStatusGridView.PageIndex = 0;
                sortOrder.Text = "Sorted By : CREATION_TIME ASC";
            }
            catch (Exception ex)
            {
                string msg  = "Error 38: Issue occured while attempting reset. Contact your system admin.";
                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                // Log the exception and notify system operators
                ExceptionUtility.LogException(ex);
                ExceptionUtility.NotifySystemOps(ex);
            }
        }



        // 'Your Jobs' DROPDOWN: POPULATE JOBSFILTER. 
        private Dictionary<int, string> populateJobsList()
        {
            try
            {
                jobsFilter.Items.Clear();
                jobsFilter.Items.Add("Your Jobs");
                
                // First, get current user id via name.
                string user = HttpContext.Current.User.Identity.Name.Split('\\').Last();
                List<int> jobIdList = new List<int>();
                Dictionary<int, string> jobsDict = new Dictionary<int, string>();
                int opID = Helper.getUserId(user);
                if (opID == 0) return new Dictionary<int, string>();

                // Check if current operator is Admin &
                // Then, get all appropriate jobs for current operator from OPERATOR_ACCESS.
                using (SqlConnection con = Helper.ConnectionObj)
                {
                    using (SqlCommand cmd = con.CreateCommand())
                    {
                        // Get operator's Admin status
                        cmd.CommandText = "SELECT ADMIN FROM OPERATOR WHERE ID = @userId";
                        cmd.Parameters.AddWithValue("@userId", opID);
                        con.Open();
                        object result = cmd.ExecuteScalar();
                        bool isAdmin = false;
                        if (result != null)
                            isAdmin = (bool)cmd.ExecuteScalar();
                        ViewState["isAdmin"] = isAdmin;
                        cmd.Parameters.Clear();

                        // If Admin, get all jobs    
                        if (isAdmin == true)
                        {
                            cmd.CommandText =   "SELECT ID, ABBREVIATION " +
                                                "FROM JOB " +
                                                "WHERE ACTIVE=1 " +
                                                "ORDER BY ABBREVIATION ASC";
                        }
                        else // Else get assigned jobs only.
                        {
                            cmd.CommandText =   "SELECT ID, ABBREVIATION " +
                                                "FROM JOB " +
                                                "JOIN OPERATOR_ACCESS ON JOB.ID=OPERATOR_ACCESS.JOB_ID " +
                                                "WHERE ACTIVE=1 AND OPERATOR_ID=@userId " +
                                                "ORDER BY ABBREVIATION ASC";
                            cmd.Parameters.AddWithValue("@userId", opID);
                        }

                        using (SqlDataReader reader = cmd.ExecuteReader())
                        {
                            if (reader.HasRows)
                            {
                                while (reader.Read())
                                {
                                    int jobId = (int)reader.GetValue(0);
                                    string jobAbb = (string)reader.GetValue(1);
                                    jobsFilter.Items.Add(jobAbb);
                                    jobsDict.Add(jobId, jobAbb);
                                }
                                jobsFilter.AutoPostBack = true;
                            }
                            else return new Dictionary<int, string>();
                        }
                    }
                }
                return jobsDict;
            }
            catch (Exception ex)
            {
                string msg  = "Error 39: Issue occured while attempting to populate jobs dropdown. Contact system admin.";
                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                // Log the exception and notify system operators
                ExceptionUtility.LogException(ex);
                ExceptionUtility.NotifySystemOps(ex);
                return new Dictionary<int, string>();
            }
        }



        // 'JOBSFILTER', 'WHO' & 'WHAT' FILTER CHANGED.
        protected void onSelectedChange(object sender, EventArgs e)
        {
            try
            {   
                sortOrder.Text = "Sorted By : CREATION_TIME ASC";
                getIndexes(jobsFilter.SelectedValue, whoFilter.SelectedValue, whenFilter.SelectedValue, whatFilter.SelectedValue);
            }
            catch (Exception ex)
            {
                string msg = "Error 40: Issue occured while attempting to process filter entries. Contact system admin.";
                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                // Log the exception and notify system operators
                ExceptionUtility.LogException(ex);
                ExceptionUtility.NotifySystemOps(ex);
            }
        }



        // 'WHEN' FILTER CHANGED.
        protected void onSelectWhen(object sender, EventArgs e)
        {
            try
            {
                if (whenFilter.SelectedValue == "allTime")
                {
                    timePanel.Visible = false;
                    getIndexes(jobsFilter.SelectedValue, whoFilter.SelectedValue, whenFilter.SelectedValue, whatFilter.SelectedValue);
                    indexeStatusGridView.Visible = true;
                    description.Visible = true;
                }
                else
                {
                    from.Text = string.Empty;
                    to.Text = string.Empty;
                    timePanel.Visible = true;
                    gridContainer.Visible = false;
                    indexeStatusGridView.Visible = false;
                    description.Visible = false;
                }
            }
            catch (Exception ex)
            {   
                string msg  = "Error 41: Issue occured while attempting to process filter entries. Contact system admin.";
                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                // Log the exception and notify system operators
                ExceptionUtility.LogException(ex);
                ExceptionUtility.NotifySystemOps(ex);
            }
        }



        // 'SUBMIT' CLICKED: DATE FIELDS ENTERED.
        protected void submit_Click(object sender, EventArgs e)
        {
            try
            {
                DateTime start = DateTime.Parse(Request.Form[from.UniqueID]);
                DateTime end = DateTime.Parse(Request.Form[to.UniqueID]).Date.AddHours(23).AddMinutes(59).AddSeconds(59);
                if (start != null) ViewState["from"] = start.Date.ToString("MM/dd/yyyy");
                if (end != null) ViewState["to"] = end.Date.ToString("MM/dd/yyyy");
                getIndexes(jobsFilter.SelectedValue, whoFilter.SelectedValue, whenFilter.SelectedValue, whatFilter.SelectedValue);
                from.Text = start.Date.ToString("MM/dd/yyyy");
                to.Text = end.Date.ToString("MM/dd/yyyy");
            }
            catch (Exception ex)
            {   
                if (ex.Message.Contains("String was not recognized as a valid DateTime"))
                {
                    ViewState["from"] = null;
                    ViewState["to"] = null;
                    
                    string msg = "All date fields required.";
                    onScreenMsg(msg, "#ff3333;", "dateRange");
                    from.Text = string.Empty;
                    to.Text = string.Empty;
                }
                else
                {
                    string msg = "Error 42: Issue occured while attempting to process filter entries. Contact system admin.";
                    ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                }
                
                // Log the exception and notify system operators
                ExceptionUtility.LogException(ex);
                ExceptionUtility.NotifySystemOps(ex);
            }
        }



        // HANDLE NEXT PAGE CLICK. FUNCTION
        protected void pageChange_Click(object sender, GridViewPageEventArgs e)
        {
            try
            {
                indexeStatusGridView.PageIndex = e.NewPageIndex;
                getIndexes(jobsFilter.SelectedValue, whoFilter.SelectedValue, whenFilter.SelectedValue, whatFilter.SelectedValue);
            }
            catch (Exception ex)
            {   
                string msg  = "Error 43:   Issue occured while attempting to change page & process filter entries. Contact system admin.";
                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                // Log the exception and notify system operators
                ExceptionUtility.LogException(ex);
                ExceptionUtility.NotifySystemOps(ex);
            }
        }



        // GET FILTERED INDEXES. 
        protected void getIndexes(string jobAbb, string who, string when, string what)
        {
            try
            {
                Page.Validate();
                if (!Page.IsValid) return;

                // First, populate 'Your Jobs' filter
                Dictionary<int, string> jobsDict = (Dictionary<int, string>)Session["jobsList"];

                string user = HttpContext.Current.User.Identity.Name.Split('\\').Last();
                int opID = Helper.getUserId(user);
                if (opID == 0 || jobsDict.Count == 0)
                {
                    description.Text = "No indexes found with the specified filter entries.";
                    description.Visible = true;
                    recordsPerPageLabel.Visible = false;
                    recordsPerPage.Visible = false;
                    sortOrder.Visible = false;
                    gridContainer.Visible = false;
                    return;
                }

                // Get job ID of selected Job
                int jobID = 0;
                foreach (var jobTuple in jobsDict)
                {
                    if (jobTuple.Value == jobAbb) jobID = jobTuple.Key;
                }

                SqlCommand cmd = new SqlCommand();
                bool isAdmin = (bool)ViewState["isAdmin"];
                string cmdString = string.Empty;
                
                //if (isAdmin == true)
                //{
                //    cmdString = "SELECT NAME, BARCODE, VALUE1, VALUE2, VALUE3, VALUE4, VALUE5, CREATION_TIME, PRINTED " +
                //                "FROM INDEX_DATA " +
                //                "INNER JOIN OPERATOR ON INDEX_DATA.OPERATOR_ID=OPERATOR.ID WHERE ";
                //}
                //else
                //{
                //    cmdString = "SELECT DISTINCT NAME, BARCODE, VALUE1, VALUE2, VALUE3, VALUE4, VALUE5, CREATION_TIME, PRINTED " +
                //                "FROM INDEX_DATA " +
                //                "INNER JOIN OPERATOR ON INDEX_DATA.OPERATOR_ID=OPERATOR.ID " +
                //                "INNER JOIN OPERATOR_ACCESS ON OPERATOR_ACCESS.JOB_ID=INDEX_DATA.JOB_ID " +
                //                "WHERE ";
                //}

                using (SqlConnection con = Helper.ConnectionObj)
                {
                    if (who == "meOnly")
                    {   
                        if (isAdmin)
                            cmdString = "SELECT NAME, BARCODE, VALUE1, VALUE2, VALUE3, VALUE4, VALUE5, VALUE6, VALUE7, VALUE8, VALUE9, VALUE10, CREATION_TIME, PRINTED " +
                                        "FROM INDEX_DATA " +
                                        "INNER JOIN OPERATOR ON INDEX_DATA.OPERATOR_ID=OPERATOR.ID WHERE INDEX_DATA.OPERATOR_ID=@opId ";
                        else
                            cmdString = "SELECT NAME, BARCODE, VALUE1, VALUE2, VALUE3, VALUE4, VALUE5, VALUE6, VALUE7, VALUE8, VALUE9, VALUE10, CREATION_TIME, PRINTED " +
                                        "FROM INDEX_DATA " +
                                        "INNER JOIN OPERATOR ON INDEX_DATA.OPERATOR_ID=OPERATOR.ID " +
                                        "INNER JOIN OPERATOR_ACCESS ON OPERATOR_ACCESS.JOB_ID=INDEX_DATA.JOB_ID " +
                                        "WHERE OPERATOR_ACCESS.OPERATOR_ID=OPERATOR.ID AND INDEX_DATA.OPERATOR_ID=@opId ";
                        if (when == "allTime")
                        {
                            timePanel.Visible = false;
                            if (what == "allSheets")
                            {
                                if (jobAbb == "Your Jobs") 
                                    cmd = new SqlCommand(cmdString, con);
                                else
                                {
                                    cmd = new SqlCommand(cmdString + "AND INDEX_DATA.JOB_ID=@jobID", con);
                                    cmd.Parameters.AddWithValue("@jobID", jobID);
                                }
                                description.Text = "Your Indexes for all Time.";
                            }
                            else if (what == "printed")
                            {
                                if (jobAbb == "Your Jobs") cmd = new SqlCommand(cmdString + "AND PRINTED=1", con);
                                else
                                {
                                    cmd = new SqlCommand(cmdString + " AND INDEX_DATA.JOB_ID=@jobID AND PRINTED=1", con);
                                    cmd.Parameters.AddWithValue("@jobID", jobID);
                                }
                                description.Text = "Your Printed Indexes for all Time.";
                            }
                            else if (what == "notPrinted")
                            {
                                if (jobAbb == "Your Jobs") cmd = new SqlCommand(cmdString + "AND PRINTED=0", con);
                                else
                                {
                                    cmd = new SqlCommand(cmdString + "AND INDEX_DATA.JOB_ID=@jobID AND PRINTED=0", con);
                                    cmd.Parameters.AddWithValue("@jobID", jobID);
                                }
                                description.Text = "Your Unprinted Indexes for all Time.";
                            }
                        }
                        else if (when == "pickRange")
                        {
                            DateTime start = DateTime.Parse(Request.Form[from.UniqueID]);
                            DateTime end = DateTime.Parse(Request.Form[to.UniqueID]).Date.AddHours(23).AddMinutes(59).AddSeconds(59);
                            if (start == default(DateTime))
                            {
                                string msg = "Please pick a start date.";
                                onScreenMsg(msg, "#ff3333;", "dateRange");
                                return;
                            }
                            if (end == default(DateTime))
                            {
                                string msg = "Please pick an end date.";
                                onScreenMsg(msg, "#ff3333;", "dateRange");
                                return;
                            }

                            if (what == "allSheets")
                            {
                                if (jobAbb == "Your Jobs") cmd = new SqlCommand(cmdString + "AND (CREATION_TIME BETWEEN @start AND @end)", con);
                                else
                                {
                                    cmd = new SqlCommand(cmdString + "AND INDEX_DATA.JOB_ID=@jobID AND (CREATION_TIME BETWEEN @start AND @end)", con);
                                    cmd.Parameters.AddWithValue("@jobID", jobID);
                                }
                                description.Text = "Your Indexes from " + start.Date.ToString("MM/dd/yyyy") + " to " + end.Date.ToString("MM/dd/yyyy") + ".";
                            }
                            else if (what == "printed")
                            {
                                if (jobAbb == "Your Jobs") cmd = new SqlCommand(cmdString + "AND PRINTED=1 AND (CREATION_TIME BETWEEN @start AND @end)", con);
                                else
                                {
                                    cmd = new SqlCommand(cmdString + "AND INDEX_DATA.JOB_ID=@jobID AND PRINTED=1 AND (CREATION_TIME BETWEEN @start AND @end)", con);
                                    cmd.Parameters.AddWithValue("@jobID", jobID);
                                }
                                description.Text = "Your Printed Indexes from " + start.Date.ToString("MM/dd/yyyy") + " to " +end.Date.ToString("MM/dd/yyyy") + ".";
                            }
                            else if (what == "notPrinted")
                            {
                                if (jobAbb == "Your Jobs") cmd = new SqlCommand(cmdString + "AND PRINTED=0 AND (CREATION_TIME BETWEEN @start AND @end)", con);
                                else
                                {
                                    cmd = new SqlCommand(cmdString + "AND INDEX_DATA.JOB_ID=@jobID AND PRINTED=0 AND (CREATION_TIME BETWEEN @start AND @end)", con);
                                    cmd.Parameters.AddWithValue("@jobID", jobID);
                                }
                                description.Text = "Your Unprinted Indexes from " +start.Date.ToString("MM/dd/yyyy")+ " to " +end.Date.ToString("MM/dd/yyyy") + ".";
                            }
                            cmd.Parameters.AddWithValue("@start", start);
                            cmd.Parameters.AddWithValue("@end", end);
                        }
                        cmd.Parameters.AddWithValue("@opId", opID);
                    }
                    else if (who == "everyone")
                    {
                        if (isAdmin)
                            cmdString = "SELECT NAME, BARCODE, VALUE1, VALUE2, VALUE3, VALUE4, VALUE5, VALUE6, VALUE7, VALUE8, VALUE9, VALUE10, CREATION_TIME, PRINTED " +
                                        "FROM INDEX_DATA " +
                                        "INNER JOIN OPERATOR ON INDEX_DATA.OPERATOR_ID=OPERATOR.ID ";
                        else
                            cmdString = "SELECT NAME, BARCODE, VALUE1, VALUE2, VALUE3, VALUE4, VALUE5, VALUE6, VALUE7, VALUE8, VALUE9, VALUE10, CREATION_TIME, PRINTED " +
                                        "FROM INDEX_DATA " +
                                        "INNER JOIN OPERATOR ON INDEX_DATA.OPERATOR_ID=OPERATOR.ID " +
                                        "WHERE INDEX_DATA.JOB_ID IN (SELECT JOB_ID FROM OPERATOR_ACCESS WHERE OPERATOR_ACCESS.OPERATOR_ID=@opId) ";
                        if (when == "allTime")
                        {
                            timePanel.Visible = false;
                            if (what == "allSheets")
                            {
                                string cmdStringShort = cmdString.Substring(0,cmdString.Length - 6);
                                if (jobAbb == "Your Jobs") cmd = new SqlCommand(cmdString, con);
                                else
                                {
                                    cmd = new SqlCommand(cmdString + "AND INDEX_DATA.JOB_ID=@jobID", con);
                                    cmd.Parameters.AddWithValue("@jobID", jobID);
                                }
                                description.Text = "Operators' Indexes for all Time.";
                            }
                            else if (what == "printed")
                            {
                                if (jobAbb == "Your Jobs") cmd = new SqlCommand(cmdString + "AND PRINTED=1", con);
                                else
                                {
                                    cmd = new SqlCommand(cmdString + "AND INDEX_DATA.JOB_ID=@jobID AND PRINTED=1", con);
                                    cmd.Parameters.AddWithValue("@jobID", jobID);
                                }
                                description.Text = "Operators' Printed Indexes for all Time.";
                            }
                            else if (what == "notPrinted")
                            {
                                if (jobAbb == "Your Jobs") cmd = new SqlCommand(cmdString + "AND PRINTED=0", con);
                                else
                                {
                                    cmd = new SqlCommand(cmdString + "AND INDEX_DATA.JOB_ID=@jobID AND PRINTED=0", con);
                                    cmd.Parameters.AddWithValue("@jobID", jobID);
                                }
                                description.Text = "Operators' Unprinted Indexes for all Time.";
                            }
                        }
                        else if (when == "pickRange")
                        {
                            DateTime start = DateTime.Parse(Request.Form[from.UniqueID]);
                            DateTime end = DateTime.Parse(Request.Form[to.UniqueID]).Date.AddHours(23).AddMinutes(59).AddSeconds(59);
                            if (start == default(DateTime))
                            {
                                string msg = "Please pick a start date.";
                                onScreenMsg(msg, "#ff3333;", "dateRange");
                                return;
                            }
                            if (end == default(DateTime))
                            {
                                string msg = "Please pick an end date.";
                                onScreenMsg(msg, "#ff3333;", "dateRange");
                                return;
                            }

                            if (what == "allSheets")
                            {
                                if (jobAbb == "Your Jobs") cmd = new SqlCommand(cmdString + "AND CREATION_TIME BETWEEN @start AND @end", con);
                                else
                                {
                                    cmd = new SqlCommand(cmdString + "AND INDEX_DATA.JOB_ID=@jobID AND CREATION_TIME BETWEEN @start AND @end", con);
                                    cmd.Parameters.AddWithValue("@jobID", jobID);
                                }
                                description.Text = "Operators' Indexes from " +start.Date.ToString("MM/dd/yyyy")+ " to " + end.Date.ToString("MM/dd/yyyy") + ".";
                            }
                            else if (what == "printed")
                            {
                                if (jobAbb == "Your Jobs") cmd = new SqlCommand(cmdString + "AND PRINTED=1 AND (CREATION_TIME BETWEEN @start AND @end)", con);
                                else
                                {
                                    cmd = new SqlCommand(cmdString + "AND INDEX_DATA.JOB_ID=@jobID AND PRINTED=1 AND (CREATION_TIME BETWEEN @start AND @end)", con);
                                    cmd.Parameters.AddWithValue("@jobID", jobID);
                                }
                                description.Text = "Operators' Printed Indexes From " +start.Date.ToString("MM/dd/yyyy")+ " to " + end.Date.ToString("MM/dd/yyyy") + ".";
                            }
                            else if (what == "notPrinted")
                            {
                                if (jobAbb == "Your Jobs") cmd = new SqlCommand(cmdString + "AND PRINTED=0 AND (CREATION_TIME BETWEEN @start AND @end)", con);
                                else
                                {
                                    cmd = new SqlCommand(cmdString + "AND INDEX_DATA.JOB_ID=@jobID AND PRINTED=0 AND (CREATION_TIME BETWEEN @start AND @end)", con);
                                    cmd.Parameters.AddWithValue("@jobID", jobID);
                                }
                                description.Text = "Operators' Unprinted Indexes from " +start.Date.ToString("MM/dd/yyyy")+ " to " + end.Date.ToString("MM/dd/yyyy") + ".";
                            }
                            cmd.Parameters.AddWithValue("@start", start);
                            cmd.Parameters.AddWithValue("@end", end);
                        }
                        cmd.Parameters.AddWithValue("@opId", opID);
                    }

                    con.Open();
                    using (SqlDataAdapter da = new SqlDataAdapter(cmd))
                    {
                        using (DataSet ds = new DataSet())
                        {
                            da.Fill(ds);
                            if (ds.Tables.Count > 0)
                            {
                                //Persist the table in the Session object.
                                Session["TaskTable"] = ds.Tables[0];

                                indexeStatusGridView.DataSource = ds.Tables[0];
                                indexeStatusGridView.DataBind();
                            }
                            con.Close();

                            // Handling of whether any index was returned from DB
                            if (indexeStatusGridView.Rows.Count == 0)
                            {
                                description.Text = "No indexes found with the specified filter entries.";
                                description.Visible = true;
                                gridContainer.Visible = false;
                                gridHeader.Visible = false;
                                recordsPerPageLabel.Visible = false;
                                recordsPerPage.Visible = false;
                                sortOrder.Visible = false;
                            }
                            else
                            {
                                description.Visible = true;
                                gridContainer.Visible = true;
                                gridHeader.Visible = true;
                                recordsPerPageLabel.Visible = true;
                                recordsPerPage.Visible = true;
                                sortOrder.Visible = true;
                                indexeStatusGridView.Visible = true;
                            }
                        }
                    }
                }
            }
            catch (Exception ex)
            {
                string msg;   
                if (ex.Message.Contains("valid DateTime"))
                {
                    msg = "All date fields required.";
                    onScreenMsg(msg, "#ff3333;", "dateRange");
                }
                else
                {
                    msg = "Error 45: Issue occured while attempting to process filter entries." ;
                    ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                    // Log the exception and notify system operators
                    ExceptionUtility.LogException(ex);
                    ExceptionUtility.NotifySystemOps(ex);
                }
            }
        }



        // RECORDS PER PAGE
        protected void onSelectedRecordsPerPage(object sender, EventArgs e)
        {
            try
            {
                if (recordsPerPage.SelectedValue != "all")
                {
                    indexeStatusGridView.AllowPaging = true;
                    indexeStatusGridView.PageSize = Int32.Parse(recordsPerPage.SelectedValue);
                }
                else indexeStatusGridView.AllowPaging = false;
                getIndexes(jobsFilter.SelectedValue, whoFilter.SelectedValue, whenFilter.SelectedValue, whatFilter.SelectedValue);
                sortOrder.Text = "Sorted By : CREATION_TIME ASC";
            }
            catch (Exception ex)
            {
                string msg = "Error 46: Issue occured while attempting to display requested number of records. Contact your system admin.";
                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                // Log the exception and notify system operators
                ExceptionUtility.LogException(ex);
                ExceptionUtility.NotifySystemOps(ex);
            }
        }



        // PREVENT LINE BREAKS IN GRIDVIEW
        protected void rowDataBound(object sender, GridViewRowEventArgs e)
        {
            try
            {
                // GIVE CUSTOM COLUMN NAMES
                if (e.Row.RowType == DataControlRowType.Header)
                {   
                    if (jobsFilter.SelectedValue != "Your Jobs")
                    {
                        // Get job ID of selected Job
                        int jobID = 0;
                        Dictionary<int, string> jobsDict = (Dictionary<int, string>)Session["jobsList"];
                        foreach (var jobTuple in jobsDict)
                        {
                            if (jobTuple.Value == jobsFilter.SelectedValue) jobID = jobTuple.Key;
                        }

                        // Retrieve job labels
                        var jobLabels = new Dictionary<string, string>();
                        using (SqlConnection con = Helper.ConnectionObj)
                        {
                            using (SqlCommand cmd = con.CreateCommand())
                            {
                                cmd.CommandText = "SELECT LABEL1, LABEL2, LABEL3, LABEL4, LABEL5, LABEL6, LABEL7, LABEL8, LABEL9, LABEL10 " +
                                                  "FROM JOB_CONFIG_INDEX " +
                                                  "WHERE JOB_ID=@jobID";
                                cmd.Parameters.AddWithValue("@jobID", jobID);
                                con.Open();
                                using (SqlDataReader reader = cmd.ExecuteReader())
                                {
                                    if (reader.HasRows)
                                    {
                                        while (reader.Read())
                                        {
                                            for (int i = 1; i <= 10; i++)
                                            {
                                                if (reader.GetValue(i - 1) != DBNull.Value)
                                                    jobLabels["label" + i] = (string)reader.GetValue(i - 1);
                                                else
                                                    jobLabels["label" + i] = string.Empty;
                                            }
                                        }
                                    }
                                }
                            }
                        }
                        ViewState["jobLabels"] = jobLabels;

                        // Rename columns headers & remove empty columns
                        for (int i = 3; i <= 12; i++)
                        {
                            if (jobLabels["label" + (i - 2)] == string.Empty)
                                e.Row.Cells[i].Visible = false;
                            else
                                ((LinkButton)e.Row.Cells[i].Controls[0]).Text = jobLabels["label" + (i - 2)].ToUpper();
                        }
                    }
                    ((LinkButton)e.Row.Cells[1].Controls[0]).Text = "OPERATOR";
                    ((LinkButton)e.Row.Cells[2].Controls[0]).Text = "INDEX";
                    ((LinkButton)e.Row.Cells[e.Row.Cells.Count - 2].Controls[0]).Text = "CREATION TIME";

                    // Set column borders & Prevent headers' line breaks
                    string colBorder = "border-left:1px solid #737373; border-right:1px solid #737373; white-space: nowrap;";
                    for (int i = 0; i < e.Row.Cells.Count; i++)
                        e.Row.Cells[i].Attributes.Add("style", colBorder);
                }

                // Set column borders & Prevent rows' line breaks
                if (e.Row.RowType == DataControlRowType.DataRow)
                {
                    // Remove empty columns
                    var jobLabels = (Dictionary<string, string>)ViewState["jobLabels"];
                    if (jobsFilter.SelectedValue != "Your Jobs")
                    {
                        for (int i = 3; i <= 12; i++)
                        {
                            if (jobLabels["label" + (i - 2)] == string.Empty)
                                e.Row.Cells[i].Visible = false;
                        }
                    }

                    // Set column borders
                    string colBorder = "border-width:1px 1px 1px 1px; border-style:solid; border-color:#cccccc; white-space: nowrap;"; 
                    for (int i=0; i<e.Row.Cells.Count; i++)
                        e.Row.Cells[i].Attributes.Add("style", colBorder);
                }

                if (e.Row.RowType == DataControlRowType.Pager)
                {
                    string colBorder = "border-left:1px solid #737373; border-right:1px solid #737373; white-space: nowrap;";
                    for (int i = 0; i < e.Row.Cells.Count; i++)
                        e.Row.Cells[i].Attributes.Add("style", colBorder);
                }
            }
            catch (Exception ex)
            {   
                string msg  = "Error 47: Issue occured while attempting to prevent line breaks within gridview. Contact system admin.";
                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                // Log the exception and notify system operators
                ExceptionUtility.LogException(ex);
                ExceptionUtility.NotifySystemOps(ex);
            }
        }



        // SORT ANY GRIDVIEW COLUMN. 
        protected void gridView_Sorting(object sender, GridViewSortEventArgs e)
        {
            try
            {
                //Retrieve the table from the session object.
                DataTable dt = Session["TaskTable"] as DataTable;

                if (dt != null)
                {
                    string label;
                    if (e.SortExpression.Contains("VALUE") && jobsFilter.SelectedValue != "Your Jobs")
                    {
                        string last = e.SortExpression.Substring(e.SortExpression.Length - 1, 1);
                        int lastInt = Convert.ToInt32(last);
                        var jobLabels = (Dictionary<string, string>)ViewState["jobLabels"];
                        label = jobLabels["label" + lastInt];
                        //string directSymbol = 
                    }
                    else if (e.SortExpression.Contains("NAME"))
                    {
                        label = "OPERATOR";
                    }
                    else if (e.SortExpression.Contains("BARCODE"))
                    {
                        label = "INDEX";
                    }
                    else
                        label = e.SortExpression;
                    //Sort the data.
                    string sortDirection = GetSortDirection(e.SortExpression);
                    dt.DefaultView.Sort = e.SortExpression + " " + sortDirection;
                    sortOrder.Text = "Sorted By : " + label.ToUpper() + "  " + sortDirection;
                    indexeStatusGridView.DataSource = Session["TaskTable"];
                    indexeStatusGridView.DataBind();
                }
            }
            catch (Exception ex)
            {
                string msg  = "Error 48: Issue occured while attempting to sort table. Contact system admin.";
                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                // Log the exception and notify system operators
                ExceptionUtility.LogException(ex);
                ExceptionUtility.NotifySystemOps(ex);
            }
        }


        // GET SORTING ORDER
        private string GetSortDirection(string column)
        {
            try
            {
                // By default, set the sort direction to ascending.
                string sortDirection = "ASC";

                // Retrieve the last column that was sorted.
                string sortExpression = ViewState["SortExpression"] as string;

                if (sortExpression != null)
                {
                    // Check if the same column is being sorted. Otherwise, the default value can be returned.
                    if (sortExpression == column)
                    {
                        string lastDirection = ViewState["SortDirection"] as string;
                        if ((lastDirection != null) && (lastDirection == "ASC"))
                        {
                            sortDirection = "DESC";
                        }
                    }
                }

                // Save new values in ViewState.
                ViewState["SortDirection"] = sortDirection;
                ViewState["SortExpression"] = column;

                return sortDirection;
            }
            catch (Exception ex)
            {
                string msg  = "Error 49: Issue occured while attempting to sort table. Contact system admin.";
                ScriptManager.RegisterStartupScript(Page, Page.GetType(), "myalert", "alert('" + msg + "');", true);
                // Log the exception and notify system operators
                ExceptionUtility.LogException(ex);
                ExceptionUtility.NotifySystemOps(ex);
                return "ASC";
            }
        }
        


        // PRINT VARIOUS MSG ON SCREEN INSTEAD OF A POPUP
        private void onScreenMsg(string msg, string color, string from)
        {
            var screenMsg = new TableCell();
            var screenMsgRow = new TableRow();
            screenMsg.Text = msg;
            screenMsg.Attributes["style"] = "color:" + color;
            screenMsgRow.Cells.Add(screenMsg);
            if (from == "dateRange") dateAlertMsg.Rows.Add(screenMsgRow);
        }

    }
}

