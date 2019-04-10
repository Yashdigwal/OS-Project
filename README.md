# OS-Project
priority Job Schedule

Student Name: YASHPAL

Student ID : 11705807

Email Address:yashdigwal66@gmail.com

GitHub Link:https://github.com/Yashdigwal/OS-Project

Code:
/*

OS PROJECT CREATED BY YASHPAL 
ON 07.04.2019
DATE OF SUBMISSION 10.04.2019

 */

// https://github.com/Yashdigwal/OS-Project

import java.util.*;

class properties
{
    String name,type;
    int startTime,requiredTime;
    properties(String name,String type,int startTime,int requiredTime)
    {
        this.name=name;
        this.type=type;
        this.startTime=startTime;
        this.requiredTime=requiredTime;
    }
}
public class OS
{
    public static void main(String[] args)
    {
        Scanner s=new Scanner(System.in);
        System.out.println("********************************************************\n");
        System.out.println("HERE 10 AM IS 0 SEC, AND 12 AM IS 120 SEC\n");
        System.out.println("********************************************************");
        System.out.println("ENTER THE NUMBER OF JOBS");
        int numJob=s.nextInt();
        PriorityQueue<properties> studentQueue=new PriorityQueue<>(new Comparator<properties>() {
            @Override
            public int compare(properties o1, properties o2) {
                return o1.startTime-o2.startTime;
            }
        });
        PriorityQueue<properties> facultyQueue=new PriorityQueue<>(new Comparator<properties>() {
            @Override
            public int compare(properties o1, properties o2) {
                return o1.startTime-o2.startTime;
            }
        });
        System.out.println("ENTER THE NAME, TYPE, START TIME, REQUIRED TIME:\n");
        System.out.println("*** FOR EXAMPLE: YASH STUDENT 17 33 ***");
        for(int i=0;i<numJob;i++)
        {
            String name1=s.next();
            String type1=s.next();
            type1=type1.toLowerCase();

            int startTime1=s.nextInt();
            int endTime1=s.nextInt();
            properties properties1=new properties(name1,type1,startTime1,endTime1);
            if(type1.equals("student") || type1.equals("s"))
            {
                studentQueue.add(properties1);
            }
            else
            {
                facultyQueue.add(properties1);
            }
        }

        int firstStartTime=-1;
        int totalSpentTime=-1;
        int runningPriority=-1;
        int currentTime1=0;
        String currentJobName="";
        int curJobEndTime=0;
        properties curJobProperties=null;
        int count1=0;
        me1: while(!studentQueue.isEmpty() || !facultyQueue.isEmpty() || runningPriority!=-1)
        {
            if(curJobEndTime==currentTime1 && runningPriority!=-1)
            {
                System.out.println("##################################################################################");
                if(runningPriority==1)
                {
                    System.out.println("Faculty "+currentJobName+" has finished at "+currentTime1+" Sec");
                }
                else
                {
                    System.out.println("Student "+currentJobName+" has finished at "+currentTime1+" Sec");
                }
                runningPriority=-1;
            }
            if(runningPriority==-1)
            {
                if (!facultyQueue.isEmpty() && facultyQueue.peek().startTime <= currentTime1)
                {
                    properties properties1 = facultyQueue.poll();

                    currentJobName = properties1.name;
                    curJobEndTime = currentTime1 + properties1.requiredTime;
                    curJobProperties=properties1;
                    System.out.println("##################################################################################");
                    System.out.println("Faculty "+currentJobName + " has started at " + currentTime1+" Sec");
                    runningPriority = 1;
                    if(count1==0)
                    {
                        firstStartTime=currentTime1;
                    }
                    count1++;
                }
                else
                {
                    if (!studentQueue.isEmpty() && studentQueue.peek().startTime <= currentTime1)
                    {
                        properties properties2 = studentQueue.poll();

                        currentJobName = properties2.name;
                        curJobEndTime = currentTime1 + properties2.requiredTime;
                        curJobProperties=properties2;
                        System.out.println("##################################################################################");
                        System.out.println("Student "+currentJobName + " has started at " + currentTime1+" Sec");
                        runningPriority = 0;
                        if(count1==0)
                        {
                            firstStartTime=currentTime1;
                        }
                        count1++;
                    }
                }
            }
            if(runningPriority==0)
            {
                if(!facultyQueue.isEmpty() && facultyQueue.peek().startTime <= currentTime1)
                {
                    curJobProperties.requiredTime=curJobEndTime-currentTime1;
                    studentQueue.add(curJobProperties);
                    System.out.println("##################################################################################");
                    System.out.println("Student "+currentJobName+" has stopped due to higher priority faculty job at "+currentTime1+" Sec");

                    properties properties3 = facultyQueue.poll();

                    currentJobName = properties3.name;
                    curJobEndTime = currentTime1 + properties3.requiredTime;
                    curJobProperties=properties3;
                    System.out.println("##################################################################################");
                    System.out.println("Faculty "+currentJobName + " has started at " + currentTime1+" Sec");
                    runningPriority = 1;
                    if(count1==0)
                    {
                        firstStartTime=currentTime1;
                    }
                    count1++;
                }
            }
            if(studentQueue.isEmpty() && facultyQueue.isEmpty() && runningPriority==-1)
            {
                totalSpentTime=currentTime1-firstStartTime;
            }
            currentTime1++;
            if(currentTime1>120)
            {
                System.out.println("************ OHH TIME LIMIT HAS OVER, SO YOU CAN NOT RUN MORE JOBS *************");
                System.out.println("************"+currentJobName+" HAS STOPPED DUE TO TIME LIMIT **************");
                totalSpentTime=currentTime1-firstStartTime;
                break me1;
            }
        }
        System.out.println("##################################################################################");
        System.out.println("******** Total Spent time = "+totalSpentTime+" Sec ********");
    }
}
