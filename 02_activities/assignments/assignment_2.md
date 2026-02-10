# Data Visualization

## Assignment 2: Good and Bad Data Visualization

### Requirements:

- Data visualizations are important tools for communication and convincing; we need to be able to evaluate the ways that data are presented in visual form to be critical consumers of information 
- To test your evaluation skills, locate two public data visualizations online, one good and one bad  
    - You can find data visualizations at https://public.tableau.com/app/discover or https://datavizproject.com/, or anywhere else you like! 
- For each visualization (good and bad):  
    - Explain (with reference to material covered up to date, along with readings and other scholarly sources, as needed) why you classified that visualization the way you did.
      ```
      This one is what I think is good: https://public.tableau.com/app/profile/kffdata/viz/InsurerParticipationonACAMarketplaces2014-2021/Dashboard1

      Rationales: I think this figure is good because it communicates the main message clearly. First, the title is clear, so I immediately know the topic and the time span, I can treat the dashboard as a comparison tool rather than a one-year snapshot. Second, the map is an appropriate choice because insurer participation is fundamentally geographic: people care about where they live, and the county-level shading lets me quickly spot areas with limited choice versus areas with more options. The legend is also quite clear. That binning is helpful because the policy interpretation is basically about choice, and using only a few categories reduces confusion.

      In addition, the dashboard is well organized. The map is the largest element, along with the pie chart (“Percent of Enrollees”) provides a quick national summary, which complements the map: the map answers "where," while the pie chart answers "how many people." The filters (year, state, insurer highlight) are also a good design because they support exploration without cluttering the visual. I can compare different years to see how participation changes, or narrow to a specific state, or highlight an insurer, all while keeping the overall layout stable. Finally, I like it shows the data source and a note about enrollment timing, because that increases transparency and reduces the risk of misunderstanding what the numbers represent. Overall, it is visually clean, interpretable, and useful for both a quick overview and deeper exploration.

      Here's what I feel is not good enough: https://public.tableau.com/app/profile/un.sdg.action.campaign/viz/MyWorld2030survey_Q2bydemographicgroup/MyWorld2030survey_Q2bydemographicgroups

      Rationales: The biggest issue in this graph is visual clutter and low discriminability. The chart is an parallel-sets style view: many colored bands cross between categories (gender, age, disability, education). When there are lots of crossings, the display quickly becomes hard to trace—line crossings and overlap are a known source of clutter in these designs, and they directly reduce readability because viewers can’t reliably follow which category connects to which. 

      A second issue is over-reliance on color. Each SDG has its own bright hue, so the graphic depends on people matching many colors across multiple panels. That is cognitively heavy, and it is also fragile for accessibility. Visualization research also warns against palettes that behave like “rainbow” schemes—many distinct hues without a perceptual ordering can be confusing and can create false visual boundaries. Using so many saturated hues at once increases the chance that the viewer sees the color noise rather than the pattern.

      Third, the encoding is not great for accurate comparison. The figure says “goals ranked by number of responses,” but the viewer is not primarily judging position on a shared axis (which is usually the most accurate perceptual channel for comparison). In practice, a simpler ranked bar chart or small multiples of ranked bars by demographic would let us compare “top concerns” much more easily.


    


      ```
    - How could this data visualization have been improved?  
      ```
     This SDG dashboard could be improved by redesigning it around the actual task—compare which SDGs rank highest within each demographic group—instead of making people to trace many crossing lines. The current parallel-sets make it hard to follow any category reliably. A more effective approach would be to replace the crossing-line view with small multiples of sorted bar charts for each demographic group (e.g., Women/Men/Other; disability; education), showing the top SDGs by count/percent. That would use position on a common scale, instead of asking viewers to decode rank through tangled connections. If the goal is to rank, then the display should visually privilege to the rank: show ranks explicitly and allow sorting by "Top 1," "Top 2," etc., rather than making rank an indirect outcome of reading a dense network.

     Color also needs to be simplified. Using 17 highly saturated SDG colors at once is overload; it can mislead or confuse because many hues are hard to distinguish consistently, especially when thin lines overlap. A practical fix is: keep SDG "brand" colors only when a single SDG is selected, but default to a neutral palette and use one highlight color for the selected goal. This would help improve accessibility.
     

     Even though I think the KFF dashboard is already strong, there are still a few ways it could be improved. First, because this is a choropleth at the county level, the dashboard should be extra explicit about whether it is mapping counts vs proportions and how classification breaks are chosen. Cartography guidance generally recommends using choropleths for standardized values rather than raw counts, and being transparent about class breaks because choices in normalization and binning can change the visual impression. KFF does include a pie chart for “percent of enrollees,” but the map itself could be strengthened by offering a toggle (e.g., “number of insurers” vs “share of enrollees in counties with one insurer”) so the user can switch between geographic availability and enrollee-weighted exposure.

     Second, I would improve interpretability and accessibility by checking color contrast and offering a colorblind-safe option, since map reading depends heavily on color discrimination. Finally, the interaction could better follow the “overview first, zoom and filter, then details-on-demand” idea by adding short annotations that summarize the main takeaway for each year like a one-sentence headline for the selected year and by showing a small trend line of the national share over time so users do not have to flip across years to see the trajectory.
    
     Reference
    1. Data Visualization: Best Practices. Statistics Canada. 2023. https://www150.statcan.gc.ca/n1/pub/89-26-0005/892600052022001-eng.htm
    2.Midway SR. Principles of Effective Data Visualization. Patterns (N Y). 2020 Nov 11;1(9):100141. doi: 10.1016/j.patter.2020.100141. PMID: 33336199; PMCID: PMC7733875.






      
      ```
- Word count should not exceed (as a maximum) 500 words for each visualization (i.e. 
300 words for your good example and 500 for your bad example)

### Why am I doing this assignment?:

- This assignment ensures active participation in the course, and assesses the learning outcomes
* Apply general design principles to create accessible and equitable data visualizations
* Use data visualization to tell a story

### Rubric:

| Component               | Scoring   | Requirement                                                 |
|-------------------------|-----------|-------------------------------------------------------------|
| Data viz classification and justification | Complete/Incomplete | - Data viz are clearly classified as good or bad<br />- At least three reasons for each classification are provided<br />- Reasoning is supported by course content or scholarly sources |
| Suggested improvements  | Complete/Incomplete | - At least two suggestions for improvement<br />- Suggestions are supported by course content or scholarly sources |

## Submission Information

🚨 **Please review our [Assignment Submission Guide](https://github.com/UofT-DSI/onboarding/blob/main/onboarding_documents/submissions.md)** 🚨 for detailed instructions on how to format, branch, and submit your work. Following these guidelines is crucial for your submissions to be evaluated correctly.

### Submission Parameters:
* Submission Due Date: `23:59 - 10/26/2025`
* The branch name for your repo should be: `assignment-2`
* What to submit for this assignment:
    * This markdown file (assignment_2.md) should be populated and should be the only change in your pull request.
* What the pull request link should look like for this assignment: `https://github.com/<your_github_username>/visualization/pull/<pr_id>`
    * Open a private window in your browser. Copy and paste the link to your pull request into the address bar. Make sure you can see your pull request properly. This helps the technical facilitator and learning support staff review your submission easily.

Checklist:
- [ ] Create a branch called `assignment-2`.
- [ ] Ensure that the repository is public.
- [ ] Review [the PR description guidelines](https://github.com/UofT-DSI/onboarding/blob/main/onboarding_documents/submissions.md#guidelines-for-pull-request-descriptions) and adhere to them.
- [ ] Verify that the link is accessible in a private browser window.

If you encounter any difficulties or have questions, please don't hesitate to reach out to our team via our Slack. Our Technical Facilitators and Learning Support staff are here to help you navigate any challenges.
