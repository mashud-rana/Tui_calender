<template>

    <div>

        <div id="top">
            <a href="https://github.com/nhn/tui.calendar">
                <img src="calender/images/img-bi.png">
            </a>
        </div>
        <div id="lnb">
            <div class="lnb-new-schedule">
                <button id="btn-new-schedule" @click.prevent="createNewSchedule"  type="button" class="btn btn-default btn-block lnb-new-schedule-btn" data-toggle="modal">
                    New schedule</button>
            </div>
            <div id="lnb-calendars" class="lnb-calendars">
                <div>
                    <div class="lnb-calendars-item">
                        <label>
                            <input class="tui-full-calendar-checkbox-square" type="checkbox" value="all" checked>
                            <span></span>
                            <strong>View all</strong>
                        </label>
                    </div>
                </div>
                <div id="calendarList" class="lnb-calendars-d1">
                </div>
            </div>
            <div class="lnb-footer">
                © NHN Corp.
            </div>
        </div>

    <div id="right">
        <div id="menu">
            <span class="dropdown">
                <button id="dropdownMenu-calendarType" class="btn btn-default btn-sm dropdown-toggle" type="button" data-toggle="dropdown"
                    aria-haspopup="true" aria-expanded="true">
                    <i id="calendarTypeIcon" class="calendar-icon ic_view_month" style="margin-right: 4px;"></i>
                    <span id="calendarTypeName">Dropdown</span>&nbsp;
                    <i class="calendar-icon tui-full-calendar-dropdown-arrow"></i>
                </button>
                <ul class="dropdown-menu" role="menu" aria-labelledby="dropdownMenu-calendarType">
                    <li role="presentation">
                        <a class="dropdown-menu-title" role="menuitem" data-action="toggle-daily">
                            <i class="calendar-icon ic_view_day"></i>Daily
                        </a>
                    </li>
                    <li role="presentation">
                        <a class="dropdown-menu-title" role="menuitem" data-action="toggle-weekly">
                            <i class="calendar-icon ic_view_week"></i>Weekly
                        </a>
                    </li>
                    <li role="presentation">
                        <a class="dropdown-menu-title" role="menuitem" data-action="toggle-monthly">
                            <i class="calendar-icon ic_view_month"></i>Month
                        </a>
                    </li>
                    <li role="presentation">
                        <a class="dropdown-menu-title" role="menuitem" data-action="toggle-weeks2">
                            <i class="calendar-icon ic_view_week"></i>2 weeks
                        </a>
                    </li>
                    <li role="presentation">
                        <a class="dropdown-menu-title" role="menuitem" data-action="toggle-weeks3">
                            <i class="calendar-icon ic_view_week"></i>3 weeks
                        </a>
                    </li>
                    <li role="presentation" class="dropdown-divider"></li>
                    <li role="presentation">
                        <a role="menuitem" data-action="toggle-workweek">
                            <input type="checkbox" class="tui-full-calendar-checkbox-square" value="toggle-workweek" checked>
                            <span class="checkbox-title"></span>Show weekends
                        </a>
                    </li>
                    <li role="presentation">
                        <a role="menuitem" data-action="toggle-start-day-1">
                            <input type="checkbox" class="tui-full-calendar-checkbox-square" value="toggle-start-day-1">
                            <span class="checkbox-title"></span>Start Week on Monday
                        </a>
                    </li>
                    <li role="presentation">
                        <a role="menuitem" data-action="toggle-narrow-weekend">
                            <input type="checkbox" class="tui-full-calendar-checkbox-square" value="toggle-narrow-weekend">
                            <span class="checkbox-title"></span>Narrower than weekdays
                        </a>
                    </li>
                </ul>
            </span>
            <span id="menu-navi">
                <button type="button" class="btn btn-default btn-sm move-today" data-action="move-today">Today</button>
                <button type="button" class="btn btn-default btn-sm move-day" data-action="move-prev">
                    <i class="calendar-icon ic-arrow-line-left" data-action="move-prev"></i>
                </button>
                <button type="button" class="btn btn-default btn-sm move-day" data-action="move-next">
                    <i class="calendar-icon ic-arrow-line-right" data-action="move-next"></i>
                </button>
            </span>
            <span id="renderRange" class="render-range"></span>
        </div>
        <div id="calendar"></div>
    </div>
    </div>
</template>

<script>
    import Calendar from 'tui-calendar'; /* ES6 */
    import "tui-calendar/dist/tui-calendar.css";
    // If you use the default popups, use this.
    import 'tui-date-picker/dist/tui-date-picker.css';
    import 'tui-time-picker/dist/tui-time-picker.css';

    export default {
        data(){
            return{
                calendar:''
            }
        },
        methods:{
            calsenderInit()
            {
                let self=this;
                self.calendar = new Calendar('#calendar', {
                useDetailPopup: true,
                defaultView: 'month',
                taskView: true,
                useCreationPopup: true,
                template: {
                    monthDayname: function(dayname) {
                    return '<span class="calendar-week-dayname-name">' + dayname.label + '</span>';
                    }
                }

                });


                self.calendar.on({

                    'beforeCreateSchedule': function(event) {
                    var startTime = event.start;
                    var endTime = event.end;
                    var title = event.title;
                    var status = event.status;
                    var isAllDay = event.isAllDay;
                    var guide = event.guide;
                    var triggerEventName = event.triggerEventName;
                    var schedule;

                    schedule = {
                            id: '1',
                            calendarId: '1',
                            title: title,
                            category: 'time',
                            dueDateClass: '',
                            start: startTime,
                            end: endTime
                        };

                    self.calendar.createSchedules([schedule]);
                }});

                // self.createNewSchudle(calendar);
            },

            createNewSchedule(event) {
                let self=this;
                var start = event.start ? new Date(event.start.getTime()) : new Date();
                var end = event.end ? new Date(event.end.getTime()) : moment().add(1, 'hours').toDate();
                self.calendar.openCreationPopup({
                    start: start,
                    end: end
                });
            },
        },
        mounted() {
            const self=this;
            self.calsenderInit();
        }
    }
</script>
