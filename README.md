# strata
strata code 
// (a) Create variable equal to exponential of lwage
generate wage = exp(lwage)

// (b) Generate new variable for annual salary
generate annual_salary = wage * 12

// (c) Collapse data to mean annual salary by year and ethnic/racial group
// Assuming black, hisp (hispanic), and other (not directly labeled, so we create it)
generate ethnic = .
replace ethnic = 1 if black == 1
replace ethnic = 2 if hisp == 1
replace ethnic = 3 if black == 0 & hisp == 0
label define ethniclbl 1 "Black" 2 "Hispanic" 3 "Other"
label values ethnic ethniclbl

collapse (mean) annual_salary, by(year ethnic)

// (d) Generate line graph of mean annual salary over time by ethnic/racial group
twoway (line annual_salary year if ethnic==1, sort lcolor(blue)) ///
       (line annual_salary year if ethnic==2, sort lcolor(red)) ///
       (line annual_salary year if ethnic==3, sort lcolor(green)), ///
       legend(label(1 "Black") label(2 "Hispanic") label(3 "Other")) ///
       title("Mean Annual Salary by Race/Ethnicity") ///
       ytitle("Mean Annual Salary") xtitle("Year")
