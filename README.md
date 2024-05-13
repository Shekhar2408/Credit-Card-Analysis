
# Credit Card and Customer Usage Analysis 

### Dashboard Link :[https://app.powerbi.com/groups/me/reports/76decb57-0827-40db-b3c5-61867e6cf551/ReportSection?experience=power-bi]


## Problem Statement
To develop a comprehensive Credit Card Weekly Dashboard that provides real-time insights to key performanse metrics and trends enabling stakeholders to monitor and analyze credit card operations effectively.

### Steps followed 

- Step 1 : Online dataset was downloaded for the FY 2023.
- Step 2: Load the .xlsx file into the Power BI platform.
- Step 3 : Open power query editor & in view tab under Data preview section, check Spotidy Database.
- Step 4 : It was observed that in none of the columns errors & empty values were present, hence started with DAX Query operarions.
- Step 5 : Various DAX operations were performed and are as folows:

-- 1) Firstly a column was added to make a combined a released date for the track-

        Date = Date([released_year],[released_month],[released_day])

--2) Measure for Maximum Streams-

        Max Streams = Max('Spotify Dataset'[streams])

--3) Measure for most streamed Track-

        Top Song Streams = 
        CALCULATE(
            sum('Spotify Dataset'[streams]),
            'Spotify Dataset'[streams] = max('Spotify Dataset'[streams]))

--4) Measure forTop streamed Vs average Streamed-

        Top Song vs Avg = 
        var x =[Top song vs avg val] RETURN
        
        if(x>0,
        FORMAT(x, "#.0%") & " " & UNICHAR(9650),
        FORMAT(x, "#.0%") & " " & UNICHAR(9660))

--5) Measure for Top streamed vs on an average how may it was played over the year-

        Top song vs avg val = 
        DIVIDE(
            [Top Song Streams] -[Average Stream per year],
            [Average Stream per year])


--6) Measure for the total number of tracks present-

        -Track = COUNT('Spotify Dataset'[track_name])

--7) Measure for the average streams per year-

        Average Stream per year = 
        CALCULATE(
            AVERAGE('Spotify Dataset'[streams]),
            ALLEXCEPT('Spotify Dataset','Date'[Year]))
    
- Step 6 :Lastly in order to analyse the tren of streamimg on monthly and daily basis, extermal tool named BRAVO was used to create Date table with al, the pre requisite details and was linked to date measure we created previously.
- Step 7 : Firstly the size of the canvas adjusted from the format section.
- Step 8 : The background of the report was created in Power Point and saved as picture.
  
  ![SNAP 1](https://github.com/Shekhar2408/Credit-Card-Analysis/assets/167020556/535859ec-a216-45d2-baa1-74315db67072)
  
- Step 9 : The picture was imported to Power BI and placed as canvas background.
- Step 10 : For data visualization stacked Bar chart was taken representing Number of streams for al the Tracks.
- Step 11 : Line Chart was added to the canvas for depicting the Released Date fo the Tracks Streamed.

  ![SNAP 2](https://github.com/Shekhar2408/Spotify-Dashboard/assets/167020556/b9c50621-8ca7-4e52-96fa-9570c083a0ec)

- Step 12 : A dedicated visual was created using the Power point as well showing the Streamers details about his/her streaming details, what was the song played and details regarding same.

 ![SNAP 3](https://github.com/Shekhar2408/Spotify-Dashboard/assets/167020556/7cbbdb88-e66d-433a-bd8a-cbd53fc19ca0)

- Step 13 : Some icons were also aded to make it even more visually understanding to the viewer.
- Step 14 : In order to get the song cover displayed when clicked on aspecific Streamed Song .
      For this we created a measure combining DAX operation and some HTML coding with the help of Chat GPT.


   Image html = 
         Var x =
         CALCULATE(
             max('Spotify Dataset'[cover_url]),
             'Spotify Dataset'[streams] = max('Spotify Dataset'[streams])
             )
          return
        
        "
        <!DOCTYPE html>
        <html lang='en'>
        <head>
        <meta charset='UTF-8'>
        <meta name='viewport' content='width=device-width, initial-scale=1.0'>
        <title>Mid Section Image with Rounded Corners</title>
        <style>
          .container {
            width: 460px; /* Adjust this width as needed */
            height: 150px; /* 16:9 aspect ratio */
            overflow: hidden;
            border-radius: 15px; /* Rounded corners */
          }
          .image {
            width: 100%;
            height: 100%;
            object-fit: cover;
            border-radius: inherit; /* Inherit parent's rounded corners */
            object-position: center
          }
        </style>
        </head>
        <body>
        <div class='container'>
          <img class='image' src='"&x&"' alt='Image'>
         </div>
        </body>
        </html>
        "

  ![SNAP 4](https://github.com/Shekhar2408/Spotify-Dashboard/assets/167020556/31817a67-2649-4dae-9967-9d42e58becc4)
  
- Step 15 : Two Flash Cards were added to the canvas-

  (a)Average Streams per year - 514.5 M

  (b) Top song Streamed Vs Avg Streamed song per year - some conditional formating was done in order to depict if thw song was streamed more or less than the avg.

- Step 16 : In the report view, Line and clusters chart was added to represent the Tracks Streamed by Days and Months.
- Step 17 : Four SLicers were also added 

  (a) Years

  (b) Date
  
  (c) Songs
  
  (d) Artists
  
- Step 18 : A new slicer was inserted to visualise songs on the basis of Tracks and no of times they streamed with their cover url with conditional formating of top 3 and Energy level.
   
![SNAP 5](https://github.com/Shekhar2408/Spotify-Dashboard/assets/167020556/58ff33a1-0f51-4d94-920f-b51ea32df5f3)

- Step 19 : For the dashboard to match with overall scheme of spotify, changed the theme of the report by advance formating and chosing colours for the colout pallete of spotify logo.
- Step 20 - Lastly clear all slicers button was added to canvas inside the refresh icon the top righ corner.
  


