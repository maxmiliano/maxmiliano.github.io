---
title: "Migrating ActiveJob to Sidekiq Job: Why and How"
subtitle: "Migration a bunch of ActiveJob classes to Sidekiq::Job"
author: Max
tags:
  - Ruby
  - Rails
  - code
  - Sidekiq
categories:
  - Tips
comments: true
related: true
date: 2024-08-06 07:32 +0200
last_modified_at: 2024-08-06 07:32 +0200
excerpt: "Explore the challenges, lessons learned, and best practices in migrating from ActiveJob to Sidekiq::Job for faster and more efficient job processing in Rails applications."
toc: true
toc_sticky: true
tagline: "Optimizing job processing with Sidekiq in Rails applications"
header:
  teaser: assets/images/sidekiq_jobs_migratino-dynamic_modern_workspace_2d07f2bf-ce67-464d-82b5-4bd2c97e9c3d.png
  overlay_image: assets/images/sidekiq_jobs_migratino-dynamic_modern_workspace_2d07f2bf-ce67-464d-82b5-4bd2c97e9c3d.png
  overlay_filter: 0.65
---
## Summary

Migrating from `ActiveJob` to `Sidekiq::Job` can significantly enhance the performance and efficiency of your application. However, it's not as simple as replacing `perform_later` with `perform_async` in your code.

Doing such migration was one of my first tasks as a Software Engineer for Regate by Qonto, providing me with a deep dive into our async processing system while understanding a good portion of the domain I am now working with. Today I'll share my experience, challenges, and best practices for a successful migration to Sidekiq.

## Why Migrate to Sidekiq?

There are several compelling reasons to migrate from `ActiveJob` to `Sidekiq::Job`. Firstly, Sidekiq offers faster job processing compared to ActiveJob.

Benchmarks, [like the one in Sidekiq GitHub's respository](https://github.com/sidekiq/sidekiq?tab=readme-ov-file#performance){:target="_blank"}, show significant performance improvements when using the Sidekiq implementation compared to the ActiveJob one from Rails.

Sidekiq utilizes Redis efficiently, which can lead to better overall application performance. Another benefit is that Sidekiq encourages the use of better practices, such as restrictions on sending entire objects directly as parameters, which can improve code quality and maintainability.

## Challenges and Pain Points

The migration process presented a few challenges and pain points.

One of the initial difficulties came from the first strategy of creating all the migration in a large pull request (PR), which made it difficult to implement, review and test comprehensively. Since we had a lot of jobs to migrate, in the middle of the process I noticed that having a single large PR with all changes was not the best approach.

Testing constraints also posed a challenge, particularly with testing retries and other functionalities, which required adjustments in testing methodology. With the ActiveJob original classes we were testing retries and failure scenarios, but with Sidekiq implementation, this was encampsulated and hard to test. After some research on how to test those, I relalized that this is not intended to be tested, because Sidekiq has its own tests for that and we don't need to test it again. But to get to this understanding took some research and time.

Another important constraint was having to create the new jobs in separate classes while keeping the original jobs untouched, as we didn't want old enqueued jobs to fail during the deployment of the change. This required careful planning and coordination to ensure a smooth transition. In a short period of time we could have both ActiveJobs and Sidekiq::Jobs running in parallel, and after some time we could remove the old ActiveJobs.

Afterwards, dividing the migration into smaller, manageable PRs facilitated reviews and feedback application but required meticulous management of the development flow. This division of work made the process more manageable and easier to convince others to review, but it often became repetitive, especially during the migration of multiple jobs.

## Lessons Learned

Throughout the migration some key lessons were learned.

Firstly, adhering to the "divide and rule" mantra by splitting the migration into smaller PRs made the process more manageable. Smaller PRs were not only easier to review and test but also facilitated quicker feedback and integration. This approach ensured that each change could be thoroughly vetted and reduced the risk of introducing new issues. Additionally, smaller PRs made it easier to engage colleagues in the review process, fostering a collaborative environment.

Secondly, leveraging Sidekiq's powerful options and community-proven methods was crucial for a reliable implementation. Here are some native Sidekiq methods we used:

* `.drain_all`: This method processes all jobs in the queue, ensuring that no pending jobs are left unprocessed. It is particularly useful during testing to simulate real-world scenarios and confirm that all jobs are handled as expected.

* `.inline do`: This block method runs all jobs inline, meaning they are executed immediately in the same thread. It is beneficial for testing, allowing developers to see the immediate effects of job execution without waiting for background processing.

* `.new.perform`: This method directly instantiates a job and performs it. It is useful for testing individual job logic in isolation, providing a straightforward way to validate that a specific job performs its intended function correctly.

These methods, combined with Sidekiq's robust configuration like `sidekiq_options`, allowed us to build a reliable and efficient job processing system while granting that all of them were well tested.

Finally, good documentation proved to be of greate value. Detailed documentation guided the migration process, ensuring consistency and helping new team members quickly get up to speed. It also served as a reference for best practices and troubleshooting, making the migration smoother and more efficient. Sidekiq has a good set of guides and best practices and we also had a good Guideline written and updated by our team's Staff and Senior Engineers, paved by previous migration of other Activejobs.

These lessons underscore the importance of strategic planning, leveraging proven tools, and maintaining thorough documentation in any significant migration project.

## Future Steps

As the migration progresses, there are some future steps to consider. It is essential to monitor and analyze performance continuously using tools like Datadog to update logs, views, monitors, and dashboards.

Cleaning up old ActiveJobs by removing unused  ActiveJob following the established guidelines is crucial to maintaining a clean codebase.

Additionally, continuing to refactor and improve the codebase will enhance performance and maintainability.

## Conclusion

The journey of migrating from `ActiveJob` to `Sidekiq::Job` still have some work to do and there are always opportunities to learn and improve. But the hardest part is done and by sharing these experiences and best practices, I hope to make the process smoother for others embarking on a similar path.

Want more Rails tips and insights like this? [Subscribe to my newsletter for regular updates.](http://eepurl.com/igx0pj){:target="_blank"}.
{: .notice--info}
