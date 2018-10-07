# My Personal Site

Simple static site using S3, Route 53, and CloudFront


# Quick Rundown

Steps taken for a quick and easy set up of the site

## S3

In S3, the site is broken down into two buckets:
 - **siteName**
 - **subDomain.siteName**

The **subDomain.siteName** is a simple redirect bucket to **siteName**, to account for the user's query preference. The **siteName** holds all the objects of the site and was the routable endpoint of Route 53 before the use of CloudFront.

Bucket Policy

    {
	  "Version": "2008-10-17",
	  "Statement": [{
	    "Sid": "AllowPublicRead",
	    "Effect": "Allow",
	    "Principal": {"AWS": "*"},
	    "Action": ["s3:GetObject"],
	    "Resource": ["arn:aws:s3:::siteName/*"]
	  }]
    }

Site logs were later added to monitor traffic via a **logs.siteName** bucket.

## Route 53

An A-record for each bucket was created with a temporary alias to my **siteName** bucket endpoint. At the time of creation I had not setup a CloudFront distribution, which is why the endpoint was a temporary alias.

Route 53 was used for the purchase and management of the **siteName** as well.

## CloudFront

Origin Domain Name: **siteName** bucket endpoint
Alternate Domain Names: **siteName** and **subDomain.siteName**
Redirect HTTP to HTTPS: enabled
Custom SSL Certificate

After the custom SSL certificates were available and CloudFront was deployed, the A-records in Route 53 were modified to the correct CloudFront alias.
