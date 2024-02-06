<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Data Comparator</title>
<style>
  table {
    border-collapse: collapse;
    width: 100%;
    margin-bottom: 20px;
  }
  th, td {
    border: 1px solid #dddddd;
    text-align: left;
    padding: 8px;
  }
  th {
    background-color: #f2f2f2;
  }
  .comparison-values {
    display: flex;
  }
  .comparison-values div {
    flex: 1;
    border-right: 1px solid #dddddd; /* Add border between old and new values */
    padding: 0 4px; /* Adjust padding for better spacing */
  }
  .comparison-values div:last-child {
    border-right: none; /* Remove border on the last column */
  }
</style>
</head>
<body>

<div id="comparisonTable"></div>

<script>
  const oldJson = {
    Name: "ABC",
    Age: 25,
    Address: {
      line1: "Line 1",
      Street: "1st Street",
      Pincode: "123456",
      NestedAddress: {
        line1: "Line 1",
        Street: "1st Street",
        Pincode: "123456",
      }
    },
    PreviousCompanies: [
      {
        Name: "Google",
        Role: "CEO"
      },
      {
        Name: "Apple",
        Role: "Developer"
      }
    ]
  };

  const newJson = {
    Name: "XYZ",
    Age: 26,
    Address: {
      line1: "Line 1",
      Street: "1st Street",
      Pincode: "567890",
      NestedAddress: {
        line1: "Line 1",
        Street: "1st Street",
        Pincode: "123456",
      }
    },
    PreviousCompanies: [
      {
        Name: "Google",
        Role: "Head of Delivery"
      },
      {
        Name: "Apple",
        Role: "Developer"
      }
    ]
  };

  function isObject(value) {
    return typeof(value) === "object" && !Array.isArray(value)
  }

  function isArray(value) {
    return Array.isArray(value)
  }

  function compareObjects(oldObj, newObj) {
    const keys = Object.keys(oldObj);

    let html = `${keys.map(key => {
                const oldValue = oldObj[key];
                const newValue = newObj[key];
                if(isArray(oldValue)) {
                    return `
                    <tr>    
                        <td colspan="3">
                                <span>${key} (Array of Objects)</span>
                                
                                    ${oldValue.map((value, index) => {
                                    return `<br/><span>Item ${index}</span><table>${compareObjects(value, newValue[index])}</table>`
                                })}
                                
                        </td>
                    </tr>`
                } else if(isObject(oldValue)) {
                    return `<tr><td colspan="3"><span >${key} (Object)</span><table>${compareObjects(oldValue, newValue)}</table></td></tr>`
                } else {
                    return `<tr><th>${key}</th><td>${oldValue}</td><td>${newValue}</td></tr>`
                }
             })}
    `;
    return html;
  }
  const comparisonHtml = `Employees
  <table>
        ${compareObjects(oldJson, newJson)}    
    </table>
  `;
  document.getElementById('comparisonTable').innerHTML = comparisonHtml;
</script>

</body>
</html>
